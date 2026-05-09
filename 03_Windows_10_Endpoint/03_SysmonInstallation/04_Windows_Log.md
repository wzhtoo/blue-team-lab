## **Windows Logs (၅) မျိုး အသေးစိတ်ရှင်းလင်းချက်**

### **၁။ Application Log**

* **မှတ်တမ်းတင်သည့်အရာ:** System ထဲမှာရှိတဲ့ Software တွေ၊ Program တွေနဲ့ Third-party applications တွေရဲ့ အလုပ်လုပ်ပုံကို မှတ်တမ်းတင်ပါတယ်။ (ဥပမာ - SQL Server Error တက်တာ၊ MS Office crash ဖြစ်တာမျိုး)။
* **CIS အမြင်:** လုံခြုံရေးထက် **Troubleshooting** (ပြဿနာရှာဖွေခြင်း) အတွက် ပိုသုံးပါတယ်။ ဒါပေမဲ့ Malware တွေက Application တစ်ခုခုကို အသုံးချပြီး Error တက်အောင် လုပ်တာမျိုးရှိရင် ဒီမှာ လာပေါ်တတ်ပါတယ်။
* **Log Level:** Information, Warning, Error။

### **၂။ Security Log**

* **မှတ်တမ်းတင်သည့်အရာ:** ဒါကတော့ လုံခြုံရေးအတွက် **အရေးကြီးဆုံး** အပိုင်းပါ။ Login ဝင်တာ (Logon/Logoff)၊ ဖိုင်တွေကို ဖွင့်ကြည့်တာ၊ Admin အခွင့်အရေး သုံးတာတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS အမြင်:** CIS က ဒီ Log အပိုင်းကို **အမြင့်ဆုံး (Maximum Size)** ထားဖို့ တိုက်တွန်းပါတယ်။ ဒီမှာပေါ်တဲ့ **Event ID** တွေကို စောင့်ကြည့်ရပါမယ်။
* **4624:** အောင်မြင်စွာ Login ဝင်ခြင်း (Successful Logon)။
* **4625:** Login ဝင်ရန် ကြိုးစားသော်လည်း ကျရှုံးခြင်း (Failed Logon)။
* **4720:** အကောင့်အသစ် ဆောက်ခြင်း။


* **မှတ်ချက်:** Audit Policy တွေကို ဖွင့်ထားမှသာ ဒီ Security Log ထဲမှာ အချက်အလက်တွေ စုံစုံလင်လင် ပေါ်မှာ ဖြစ်ပါတယ်။

### **၃။ Setup Log**

* **မှတ်တမ်းတင်သည့်အရာ:** Windows Update လုပ်တဲ့အခါ၊ ဒါမှမဟုတ် Windows ရဲ့ Feature အသစ်တွေ ထည့်သွင်းတဲ့အခါ ဖြစ်ပေါ်တဲ့ အပြောင်းအလဲတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS အမြင်:** System Hardening လုပ်တဲ့အခါ Windows က နောက်ဆုံးထွက် Security Patch တွေ တကယ်တက်သွားရဲ့လား (Successful update) ဆိုတာကို စစ်ဆေးဖို့ သုံးပါတယ်။

### **၄။ System Log**

* **မှတ်တမ်းတင်သည့်အရာ:** Windows OS ရဲ့ အတွင်းပိုင်း Component တွေ (ဥပမာ - Drivers, Services, Hardware Errors) ရဲ့ အခြေအနေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS အမြင်:** အရေးကြီးတဲ့ System Service တွေ ရုတ်တရက် ရပ်သွားတာ (Service failure) သို့မဟုတ် စက်အလိုအလျောက် Restart ကျသွားတာမျိုးတွေကို ဒီမှာ ခြေရာခံရပါတယ်။
* **ဥပမာ:** "The Print Spooler service entered the stopped state" ဆိုတာမျိုးကို ဒီမှာ မြင်ရမှာပါ။

### **၅။ Forwarded Events Log**

* **မှတ်တမ်းတင်သည့်အရာ:** ဒါကတော့ အခြားသော ကွန်ပျူတာတွေဆီကနေ လှမ်းပို့လိုက်တဲ့ Event Log တွေကို စုစည်းသိမ်းဆည်းတဲ့ နေရာဖြစ်ပါတယ်။
* **CIS အမြင်:** ဒါဟာ **Centralized Logging** စနစ်အတွက်ပါ။ ဥပမာ - ကုမ္ပဏီမှာ PC အလုံး ၁၀၀ ရှိရင်၊ စက်တိုင်းကို လိုက်ကြည့်မယ့်အစား PC အားလုံးရဲ့ Log တွေကို Server တစ်လုံးတည်းဆီ စုပို့ခိုင်းတဲ့အခါ ဒီနေရာမှာ လာပေါ်ပါတယ်။
* **အကျိုးကျေးဇူး:** ဟက်ကာက စက်တစ်လုံးထဲကို ဖောက်ထွင်းပြီး Log တွေ ဖျက်ပစ်လိုက်ရင်တောင် Forwarded Events ကြောင့် Server ပေါ်မှာ မှတ်တမ်းကျန်နေမှာ ဖြစ်ပါတယ်။

---

## **Windows Logs Summary**

| Log Name | Primary Content | Security Importance (CIS) |
| --- | --- | --- |
| **Application** | Software events & errors | Medium (Malware behavior) |
| **Security** | Auditing, Login, Permissions | **Highest (Forensics)** |
| **Setup** | Windows Updates & Installation | Low-Medium (Compliance) |
| **System** | OS Components & Drivers | High (Availability & Stability) |
| **Forwarded** | Logs from other computers | High (Centralized Monitoring) |
---

### **Security Tips:**

1. **Retention Policy:** CIS စံနှုန်းအရ ဒီ Log တွေ အားလုံးကို အနည်းဆုံး **Archive the log when full** (သို့မဟုတ်) SIEM (ဥပမာ- Splunk, ELK) တစ်ခုခုဆီ ပို့ထားဖို့ အကြံပြုပါတယ်။
2. **Size Matters:** Security နဲ့ System log တွေဟာ အချက်အလက် အရမ်းများတတ်တဲ့အတွက် Default size ထက် အများကြီး ပိုတိုးထားသင့်ပါတယ်။
3. **Audit Awareness:** Security Log ထဲမှာ ဘာမှမပေါ်ဘူးဆိုရင် `gpedit.msc` ထဲက Audit Policy တွေကို ဖွင့်ဖို့ မေ့နေတာ ဖြစ်နိုင်ပါတယ်။
