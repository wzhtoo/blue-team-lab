# Management Guide

Sysmon (System Monitor) ဟာ Windows ရဲ့ ပုံမှန် Event Log တွေထက် ပိုမိုအသေးစိတ်တဲ့ (ဥပမာ - Network connection တွေ၊ File creation တွေနဲ့ Process တည်ဆောက်မှုတွေ) ကို မှတ်တမ်းတင်ပေးနိုင်တဲ့အတွက် **Blue Team Monitoring** မှာ မရှိမဖြစ်လိုအပ်ပါတယ်။

## **Sysmon Log Monitoring & Management Guide**

Sysmon ကို Install လုပ်ပြီးနောက် မှတ်တမ်းတွေကို ကြည့်ရှုဖို့ `eventvwr.msc` (Event Viewer) ကို အသုံးပြုရပါတယ်။

### **၁။ Sysmon Operational Logs ကို Setup လုပ်ခြင်း**

၁။ `Win + R` ကိုနှိပ်ပြီး **`eventvwr.msc`** လို့ ရိုက်ထည့်ပါ။
၂။ ဘယ်ဘက်ခြမ်းက လမ်းကြောင်းအတိုင်း သွားပါ-

> **Applications and Services Logs** > **Microsoft** > **Windows** > **Sysmon** > **Operational**
> ၃။ **Operational** ကို Right-click နှိပ်ပြီး **Properties** ကို သွားပါ။

**CIS စံနှုန်းနှင့် ချိန်ညှိရန် အရေးကြီးသော Settings များ:**

* **Maximum log size (KB):** CIS ကတော့ အနည်းဆုံး `1,024,000 KB` (1 GB) သို့မဟုတ် ထိုထက်ပို၍ ထားရှိရန် အကြံပြုပါတယ်။ (မှတ်တမ်းတွေ အလွယ်တကူ ပျောက်မသွားစေရန် ဖြစ်ပါတယ်)။
* **Retention Method:** `Overwrite events as needed` ကို ရွေးချယ်ထားခြင်းဖြင့် Disk Space ပြည့်သွားတဲ့အခါ အဟောင်းဆုံးမှတ်တမ်းတွေကို အလိုအလျောက် ဖျက်ပေးသွားမှာပါ။

---

### **၂။ Log Entry များကို အသေးစိတ် လေ့လာခြင်း (Analysis)**

Log entry တစ်ခုကို Click နှိပ်လိုက်တဲ့အခါ အောက်ပါ **Event IDs** တွေကို အဓိကထားပြီး စောင့်ကြည့်ရပါမယ် (ဒါဟာ CIS ရဲ့ Monitoring အပိုင်းမှာ ပါဝင်ပါတယ်)-

* **Event ID 1 (Process Creation):** ဘယ် Program ကို ဘယ်သူက ဘယ် Command line နဲ့ Run လိုက်သလဲဆိုတာ ပြပါတယ်။ (Malware တွေကို ခြေရာခံဖို့ အဓိကဖြစ်ပါတယ်)။
* **Event ID 3 (Network Connection):** စက်ထဲကနေ အပြင်က IP တွေဆီ လှမ်းချိတ်တာကို ပြပါတယ်။ (C2 Server တွေကို ရှာဖွေဖို့ သုံးပါတယ်)။
* **Event ID 11 (File Create):** ဖိုင်အသစ်တွေ ဖန်တီးတာကို မှတ်တမ်းတင်ပါတယ်။ (Ransomware တွေကို စစ်ဆေးဖို့ အသုံးဝင်ပါတယ်)။

**Details Tab:** `Friendly View` ထက် `XML View` ကို ကြည့်ခြင်းက ပိုမိုတိကျတဲ့ အချက်အလက် (ဥပမာ - Hash values) တွေကို ရရှိစေပါတယ်ခင်ဗျာ။
---

### **၃။ Log များကို Clear လုပ်ပြီး အသစ်ပြန်စခြင်း (Resetting Logs)**

Analysis လုပ်ရတာ ရှုပ်ထွေးနေလို့ ဒါမှမဟုတ် စမ်းသပ်မှုအသစ် ပြန်စချင်လို့ Log တွေကို ဖျက်ချင်တယ်ဆိုရင်-

**နည်းလမ်း (၁) - Event Viewer မှတစ်ဆင့်:**
၁။ `Operational` ကို Right-click နှိပ်ပါ။
၂။ **`Clear Log...`** ကို နှိပ်ပါ။
၃။ **`Save and Clear`** (မှတ်တမ်းဟောင်းကို သိမ်းပြီးမှဖျက်ရန်) သို့မဟုတ် **`Clear`** (တန်းဖျက်ရန်) ကို ရွေးချယ်ပါ။

**နည်းလမ်း (၂) - PowerShell မှတစ်ဆင့် (Automation အတွက် ပိုကောင်းသည်):**

```powershell
# Sysmon Operational Log ကို Clear လုပ်ခြင်း
Clear-EventLog -LogName "Microsoft-Windows-Sysmon/Operational"
```

*(မှတ်ချက် - ဤ Command ကို Admin အခွင့်အရေးဖြင့် Run ရပါမည်။)*

---

### **CIS Compliance Tip for Repo:**

> **Important:** CIS စံနှုန်းအရ Log တွေကို Clear လုပ်ခြင်းဟာ တိုက်ခိုက်သူတွေ သက်သေဖျောက်တဲ့ နေရာမှာ သုံးလေ့ရှိတဲ့အတွက်၊ **"Log Clear"** ဖြစ်သွားတဲ့ ဖြစ်စဉ် (Event ID 1102 သို့မဟုတ် 104) ကိုလည်း Audit Policy မှာ သေချာစောင့်ကြည့်နေရမှာ ဖြစ်ပါတယ်။

---

### အနှစ်ချုပ် (Quick Setup Summary)**

| Task | Configuration | CIS Recommendation |
| --- | --- | --- |
| **Log Location** | Sysmon/Operational | Monitoring Essential |
| **Max Log Size** | 1024000 KB (1 GB) | High Retention |
| **Primary Events** | Event ID 1, 3, 11 | Full Visibility |
| **Log Management** | Clear with Archive | Forensic Integrity |

