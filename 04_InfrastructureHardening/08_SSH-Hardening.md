## SSH Hardening (Secure Shell)

SSH ကို Hardening လုပ်တဲ့အခါ `/etc/ssh/sshd_config` ဆိုတဲ့ configuration ဖိုင်မှာ ပြင်ဆင်ရပါတယ်။ တစ်ခုချင်းစီရဲ့ အလုပ်လုပ်ပုံနဲ့ ဘာကြောင့်လုပ်သင့်တယ်ဆိုတာကို အောက်ပါအတိုင်း လေ့လာနိုင်ပါတယ်-

### **၁။ Disable Root Login (Standard Practice)**

Root user ဆိုတာ system ထဲမှာ အကန့်အသတ်မရှိ အာဏာရှိတဲ့ user ဖြစ်လို့ တိုက်ခိုက်သူတွေရဲ့ ပထမဆုံး ပစ်မှတ်ဖြစ်ပါတယ်။

* **Command:** `PermitRootLogin no`
* **ဘာကြောင့်လဲ:** Root နဲ့ တိုက်ရိုက်ဝင်ခွင့်ကို ပိတ်ပြီး၊ သာမန် user (ဥပမာ- `defender`) နဲ့ဝင်ကာ လိုအပ်မှ `sudo` သုံးခြင်းက Brute-force တိုက်ခိုက်မှုကို သိသိသာသာ လျှော့ချပေးပါတယ်။

### **၂။ Change Default Port (Obsecurity)**

SSH ရဲ့ မူလ Port ဟာ **22** ဖြစ်ပါတယ်။ တိုက်ခိုက်သူတွေရဲ့ automated bots တွေဟာ port 22 ကိုပဲ အမြဲရှာဖွေနေကြတာပါ။

* **Command:** `Port 2222` (သို့မဟုတ် အခြား port နံပါတ်တစ်ခုခု)
* **ဘာကြောင့်လဲ:** ဒါဟာ လုံခြုံရေးကို အပြည့်အဝ မပေးနိုင်ပေမဲ့ bot attacks တွေရဲ့ ၈၀% ကျော်ကို သက်သာစေပါတယ်။

### **၃။ Use SSH Key-based Authentication (Highly Recommended)**

Password သုံးပြီး ဝင်တာဟာ ခန့်မှန်းရလွယ်ကူတဲ့အတွက် အန္တရာယ်ရှိပါတယ်။ SSH Key (Public/Private Key) က ပိုမိုခိုင်မာပါတယ်။

* **Command:** `PasswordAuthentication no`
* **ဘာကြောင့်လဲ:** Password နဲ့ဝင်ခွင့်ကို လုံးဝပိတ်လိုက်တဲ့အတွက် SSH Key မရှိတဲ့သူဟာ ဘယ်လိုမှ ဝင်လို့မရတော့ပါဘူး။

### **၄။ Limit Login Attempts & Session Timeout**

တိုက်ခိုက်သူတွေ ဆက်တိုက် Password ခန့်မှန်းတာမျိုး မလုပ်နိုင်အောင် ကန့်သတ်ခြင်း ဖြစ်ပါတယ်။

* **MaxAuthTries 3:** ၃ ကြိမ်ထက်ပိုမှားရင် connection ဖြတ်ပစ်ခြင်း။
* **ClientAliveInterval 300:** ၅ မိနစ်အတွင်း ဘာမှမလုပ်ဘဲ ငြိမ်နေရင် connection ကို အလိုအလျောက် ပိတ်ချခြင်း (Idle timeout)။
---

### လက်တွေ့ပြုပြင်ပြောင်းလဲပုံ (Step-by-Step)

terminal မှာ အောက်ပါအတိုင်း လုပ်ဆောင်နိုင်ပါတယ်-

1. **Config ဖိုင်ကို Backup အရင်ယူပါ:**
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

2. **ဖိုင်ကို Nano editor နဲ့ ဖွင့်ပြင်ပါ:**
```bash
sudo nano /etc/ssh/sshd_config
```

3. **အောက်ပါ Line များကို ရှာဖွေ၍ ပြင်ဆင်ပါ:**
```text
PermitRootLogin no
MaxAuthTries 3
PasswordAuthentication no   # (SSH key setup လုပ်ပြီးမှသာ no လုပ်ရန်)
Port 2222                  # (သတိပြုရန်: firewall မှာ port 2222 ကို အရင်ဖွင့်ရမည်)
```

4. **SSH Service ကို Restart ချပါ:**
```bash
sudo systemctl restart ssh
```

### Summary
| Hardening Feature | Configuration | Security Benefit |
| --- | --- | --- |
| **Root Access** | `PermitRootLogin no` | Prevents direct administrative compromise. |
| **Authentication** | `PasswordAuthentication no` | Eliminates password guessing attacks. |
| **Brute Force** | `MaxAuthTries 3` | Limits automated password cracking. |
| **Idle Timeout** | `ClientAliveInterval 300` | Closes unused sessions to save resources. |
---

