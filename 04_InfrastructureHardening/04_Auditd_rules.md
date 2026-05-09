## Lab Guide: Implementing CIS Benchmark v3.0.0 for Auditd Hardening

ဒီ Lab မှာ ကျွန်ုပ်တို့အသုံးပြုထားတဲ့ Rules တွေဟာ [CIS_Ubuntu_Linux_22.04_LTS_Benchmark_v3.0.0.pdf](CIS_Ubuntu_Linux_22.04_LTS_Benchmark_v3.0.0.pdf) ထဲက **Section 4.1 (Logging and Auditing)** ကို တိုက်ရိုက်အကိုးအကား ပြုထားတာဖြစ်ပါတယ်။

### **အဆင့် (၁) - CIS Configuration File ဖန်တီးခြင်း**

CIS Level 2 စံနှုန်းတွေကို သီးသန့်ထားရှိဖို့အတွက် အောက်ပါ command နဲ့ file အသစ်ဆောက်ပါ။

```bash
sudo nano /etc/audit/rules.d/cis-hardening.rules
```

### **အဆင့် (၂) - CIS Rules များထည့်သွင်းခြင်း (Reference: CIS Section 4.1)**

အောက်ပါ rules များသည် CIS Level 2 (Server & Workstation) နှစ်ခုလုံးအတွက် လိုအပ်ချက်များ ဖြစ်သည်-

```bash
## Reference: CIS Ubuntu Linux 22.04 LTS Benchmark v3.0.0
## Section 4.1.3 - 4.1.17

# 4.1.3: စက်၏ အချိန်ပြောင်းလဲမှုကို စောင့်ကြည့်ခြင်း (Time/Date change)
-a always,exit -F arch=b64 -S adjtimex -S settimeofday -k time-change
-a always,exit -F arch=b64 -S clock_settime -k time-change
-w /etc/localtime -p wa -k time-change

# 4.1.4: User/Group အချက်အလက်ပြောင်းလဲမှုကို စောင့်ကြည့်ခြင်း (Identity change)
-w /etc/group -p wa -k identity
-w /etc/passwd -p wa -k identity
-w /etc/gshadow -p wa -k identity
-w /etc/shadow -p wa -k identity
-w /etc/security/opasswd -p wa -k identity

# 4.1.5: Network Environment ပြောင်းလဲမှုကို စောင့်ကြည့်ခြင်း (System Locale)
-a always,exit -F arch=b64 -S sethostname -S setdomainname -k system-locale
-w /etc/issue -p wa -k system-locale
-w /etc/issue.net -p wa -k system-locale
-w /etc/hosts -p wa -k system-locale
-w /etc/network -p wa -k system-locale

# 4.1.10: Privilege Escalation (sudo သုံးစွဲမှု) ကို စောင့်ကြည့်ခြင်း
-w /etc/sudoers -p wa -k scope
-w /etc/sudoers.d/ -p wa -k scope

# 4.1.13: File ဖျက်ခြင်း သို့မဟုတ် နာမည်ပြောင်းခြင်း (File Deletion)
-a always,exit -F arch=b64 -S unlink -S unlinkat -S rename -S renameat -k delete

# 4.1.17: Audit Configuration ကို Lock ချခြင်း (Immutability)
# Note: တစ်ခါ lock ချပြီးရင် စက်ကို restart မချမချင်း rule ပြင်မရတော့ပါ။
-e 2
```

### **အဆင့် (၃) - Rules များ အသက်ဝင်အောင် ပြုလုပ်ခြင်း**

```bash
# Configuration ကို ပြန်လည် Reload လုပ်ခြင်း
sudo service auditd restart
```

### **အဆင့် (၄) - CIS Compliance စစ်ဆေးခြင်း**

PDF ထဲမှာပါတဲ့ rules တွေ တကယ်ဝင်သွားပြီလားဆိုတာ အောက်ပါအတိုင်း စစ်ဆေးနိုင်ပါတယ်။

```bash
# လက်ရှိ active ဖြစ်နေသော rules များကို စာရင်းထုတ်ကြည့်ခြင်း
sudo auditctl -l
```

### **အဆင့် (၅) - Rules များအား Kernel ထဲသို့ Load ပြုလုပ်ခြင်း**

Linux Audit system တွင် `/etc/audit/rules.d/` အောက်ရှိ rules ဖိုင်အသစ်များကို Kernel ထဲသို့ အထိရောက်ဆုံးနှင့် အမြန်ဆုံး Load လုပ်ရန်အတွက် အောက်ပါ command ကို အသုံးပြုပါသည်။

```bash
# CIS rules များကို စုစည်း၍ Kernel ထဲသို့ တိုက်ရိုက် Load လုပ်ခြင်း
sudo augenrules --load
```
> **Pro-Tip:** `augenrules --load` ကို အသုံးပြုခြင်းဖြင့် service တစ်ခုလုံးကို restart ချရန်မလိုဘဲ ကျွန်ုပ်တို့ အသစ်ထည့်လိုက်သော CIS hardening rules များကို ချက်ချင်း အသက်ဝင်စေပါသည်။

---

### **အဆင့် (၆) - CIS Compliance စစ်ဆေးခြင်း**

ယခုအခါ PDF ထဲမှ သတ်မှတ်ထားသော rules များသည် Kernel ထဲတွင် အမှန်တကယ် Active ဖြစ်နေပြီလားဆိုသည်ကို အောက်ပါအတိုင်း စစ်ဆေးနိုင်ပါသည်-

```bash
# လက်ရှိ Active ဖြစ်နေသော Audit rules များကို စာရင်းထုတ်ကြည့်ခြင်း
sudo auditctl -l
```