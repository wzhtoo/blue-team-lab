# **Event IDs (Event IDs)** 

Blue Team (Security Analyst) တွေအတွက် တကယ့်ကို ရွှေတွင်းပါပဲ။ ဘယ်အချက်အလက်ကို ဘယ် ID နဲ့ ကြည့်ရမလဲဆိုတာ သိထားရင် တိုက်ခိုက်သူရဲ့ ခြေရာကို မိနစ်ပိုင်းအတွင်း ရှာတွေ့နိုင်ပါတယ်။

---

## **Advanced Audit Event IDs Guide (For Blue Team)**

### **၁။ Logon & Logoff (ဝင်ပေါက်/ထွက်ပေါက် စောင့်ကြည့်ခြင်း)**

ဒါဟာ ကျူးကျော်သူ ရှိ၊ မရှိ စစ်ဆေးဖို့ အခြေခံအကျဆုံး အပိုင်းပါ။

* **Event ID 4624 (Successful Logon):** အောင်မြင်စွာ Login ဝင်နိုင်ခဲ့ခြင်း။ (Logon Type ကို ကြည့်ခြင်းဖြင့် စက်ရှေ့ကဝင်တာလား၊ Remote က ဝင်တာလား ခွဲခြားနိုင်ပါတယ်)။
* **Event ID 4625 (Failed Logon):** Password မှားရိုက်ခြင်း သို့မဟုတ် အကောင့်မရှိဘဲ ဝင်ရန်ကြိုးစားခြင်း။ (Brute Force တိုက်ခိုက်မှုကို သိနိုင်ပါတယ်)။
* **Event ID 4648 (Logon using explicit credentials):** တိုက်ခိုက်သူက `Runas` command သုံးပြီး တခြားသူရဲ့ username/password နဲ့ အလုပ်လုပ်ဖို့ ကြိုးစားတဲ့အခါ ပေါ်ပါတယ်။

---

### **၂။ Process Tracking (အလုပ်လုပ်ပုံ စောင့်ကြည့်ခြင်း)**

Malware တွေ၊ Ransomware တွေဟာ process တွေကို အသုံးချပြီး အလုပ်လုပ်တာ ဖြစ်ပါတယ်။

* **Event ID 4688 (Process Creation):** Program တစ်ခု စတင်ဖွင့်လိုက်တိုင်း မှတ်တမ်းတင်ပါတယ်။
* *Tip:* **"Include command line in process creation events"** ကို ဖွင့်ထားရင် ဟက်ကာ ဘယ်လို command တွေ ရိုက်သွားလဲဆိုတာ အသေးစိတ် မြင်ရပါလိမ့်မယ်။


* **Event ID 4689 (Process Termination):** Program တစ်ခု ပိတ်သွားခြင်း။

---

### **၃။ Account Management (အကောင့်စီမံခန့်ခွဲမှု)**

Admin အခွင့်အရေးကို ခိုးယူဖို့ ကြိုးစားတာတွေကို ဒီမှာ ခြေရာခံပါတယ်။

* **Event ID 4720:** အကောင့်အသစ် ဖန်တီးခြင်း။
* **Event ID 4722:** အကောင့်တစ်ခုကို ပြန်ဖွင့်ခြင်း (User account enabled)။
* **Event ID 4728/4732/4756:** User တစ်ယောက်ကို **Administrators Group** ထဲ ထည့်လိုက်ခြင်း (ဒါဟာ အရမ်းအရေးကြီးတဲ့ Alert ပါ)။
* **Event ID 4724:** Password reset ချရန် ကြိုးစားခြင်း။

---

### **၄။ Object Access (ဖိုင်နှင့် ဒေတာ စောင့်ကြည့်ခြင်း)**

အရေးကြီးဒေတာတွေ ခိုးယူခံရမှု (Data Exfiltration) ကို စစ်ဆေးဖို့ပါ။

* **Event ID 4663:** ဖိုင်တစ်ခုကို ဝင်ရောက်ကြည့်ရှုခြင်း၊ ပြင်ဆင်ခြင်း သို့မဟုတ် ဖျက်ပစ်ခြင်း။ (ဘယ်သူက ဘယ်ဖိုင်ကို ဖျက်သွားလဲဆိုတာ သိနိုင်ပါတယ်)။
* **Event ID 4657:** Registry Key တစ်ခုကို ပြောင်းလဲခြင်း။ (Malware တွေက စက်တက်တိုင်း သူတို့ပါဝင်လာအောင် Registry မှာ လာပြင်တတ်ပါတယ်)။

---

### **၅။ Privilege Use (အခွင့်အရေး အသုံးပြုမှု)**

* **Event ID 4672 (Special Privileges Assigned):** Admin အခွင့်အရေးနဲ့ Login ဝင်လာတဲ့အခါတိုင်း ပေါ်ပါတယ်။ "Super User" တွေ ဘာလုပ်နေလဲဆိုတာ စောင့်ကြည့်ဖို့ပါ။

---

### **၆။ System Events (စက်၏ အခြေအနေ)**

* **Event ID 1102:** **Security Log များကို ဖျက်ပစ်ခြင်း (Log Cleared)**။ (ဒါဟာ တိုက်ခိုက်သူက သက်သေဖျောက်ဖို့ ကြိုးစားတဲ့ အထင်ရှားဆုံး လက္ခဏာဖြစ်လို့ တွေ့တာနဲ့ ချက်ချင်း စစ်ဆေးရပါမယ်)။
* **Event ID 4616:** System Time ကို ပြောင်းလဲခြင်း။

---

### **Advanced IDs Quick Reference Table (For Repo)**

| Event ID | Description | Priority |
| --- | --- | --- |
| **4625** | Failed Logon (Brute Force Indicator) | **High** |
| **4688** | New Process Created (Malware Tracking) | **High** |
| **4720** | New User Created (Backdoor Check) | **Critical** |
| **4732** | Added to Local Admin Group | **Critical** |
| **1102** | Audit Log Cleared (Anti-Forensics) | **Immediate Alert** |
| **4663** | File Access/Delete | **Medium/High** |

---

### **(SIEM Integration):**

> "လက်တွေ့ လုပ်ငန်းခွင်မှာ ဒီ Event IDs တွေကို Event Viewer မှာ တစ်ခုချင်း လိုက်ဖတ်နေဖို့ မဖြစ်နိုင်ပါဘူး။ ဒါကြောင့် ဒီ ID တွေကို **SIEM (Security Information and Event Management)** software တွေဆီ Forward လုပ်ပြီး၊ အရေးကြီးတဲ့ ID တွေ (ဥပမာ- 1102, 4732) ပေါ်လာတာနဲ့ ဖုန်းဆီကို Alert ပို့အောင် Setup လုပ်ထားရမှာ ဖြစ်ပါတယ်။"

---