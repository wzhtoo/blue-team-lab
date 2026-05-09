## **၁။ Sysmon Logs Analysis (whoami နှင့် ping 8.8.8.8)**

PowerShell မှာ Command တွေရိုက်လိုက်တဲ့အခါ Sysmon က အောက်ပါအတိုင်း မှတ်တမ်းတင်ပေးပါတယ်။

### **(က) `whoami` ရိုက်လိုက်တဲ့အခါ (Event ID 1: Process Creation)**

Sysmon က `whoami.exe` ဆိုတဲ့ process အသစ်တစ်ခု စတင်လိုက်တာကို မှတ်တမ်းတင်ပါမယ်။

* **ProcessCommandline:** `whoami`
* **User:** လက်ရှိ Login ဝင်ထားတဲ့ User နာမည်။
* **ParentCommandLine:** `powershell.exe` (ဘာလို့လဲဆိုတော့ PowerShell ထဲကနေ ခေါ်လိုက်လို့ ဖြစ်ပါတယ်)။
* **Hashes:** `whoami.exe` ရဲ့ SHA256 hash ပါ ပါလာမှာဖြစ်လို့ ဒါဟာ တကယ့် Windows ဖိုင်အစစ်လား၊ Malware က နာမည်လိမ်ထားတာလားဆိုတာ စစ်လို့ရပါတယ်။

### **(ခ) `ping 8.8.8.8` ရိုက်လိုက်တဲ့အခါ (Event ID 1 နှင့် Event ID 3)**

ဒီနေရာမှာ မှတ်တမ်းနှစ်ခု ထွက်လာပါလိမ့်မယ်-

1. **Event ID 1 (Process Creation):** `ping.exe` ကို run လိုက်တဲ့မှတ်တမ်း။
2. **Event ID 3 (Network Connection):** ဒါက ပိုအရေးကြီးပါတယ်။ `ping.exe` ကနေ Google ရဲ့ DNS ဖြစ်တဲ့ `8.8.8.8` (Destination IP) ဆီကို Port 0 (ICMP) သုံးပြီး လှမ်းချိတ်တာကို မှတ်တမ်းတင်ပါမယ်။
* **Source IP:** သင့်စက်ရဲ့ IP
* **Destination IP:** 8.8.8.8
* **Protocol:** icmp
---

## **၂။ Sysmon Properties Setup (CIS Compliance)**

`eventvwr.msc` > `Sysmon/Operational` ကို Right-click နှိပ်ပြီး **Properties** မှာ အောက်ပါအတိုင်း ညှိနှိုင်းနိုင်ပါတယ်-

### **(က) Maximum log size (KB)**

* **လုပ်ဆောင်ချက်:** Log ဖိုင် ဘယ်လောက်အထိ အကြီးခံမလဲဆိုတာပါ။
* **CIS Recommendation:** အနည်းဆုံး **1,024,000 KB (1 GB)** ထားရန်။
* **အကျိုးကျေးဇူး:** Log size သေးရင် မှတ်တမ်းအဟောင်းတွေ ခဏလေးနဲ့ ပျောက်ပျောက်သွားတတ်လို့ Forensics လုပ်ရခက်စေပါတယ်။

### **(ခ) Retention Method (Retention Policy)**

ဒီနေရာမှာ ရွေးချယ်စရာ ၃ ခု ရှိပါတယ်-

1. **Overwrite events as needed (Oldest events first):**
* **အလုပ်လုပ်ပုံ:** Log size ပြည့်သွားရင် အဟောင်းတွေကို ဖျက်ပြီး အသစ်တွေ ထပ်ရေးပါတယ်။
* **CIS ချိန်ညှိချက်:** သာမန် Workstation တွေအတွက် ဒါကို သုံးနိုင်ပါတယ်။ ဒါပေမဲ့ Log size ကိုတော့ ကြီးကြီးထားရပါမယ်။


2. **Archive the log when full, do not overwrite events:**
* **အလုပ်လုပ်ပုံ:** Log size ပြည့်သွားရင် `Archive-Sysmon-YYYY-MM-DD.evtx` ဆိုပြီး ဖိုင်အသစ်ခွဲပြီး အလိုအလျောက် သိမ်းပေးပါတယ်။
* **CIS Recommendation:** **ဒါဟာ CIS ရဲ့ အကြံပြုချက်နဲ့ အကိုက်ညီဆုံးပါ။** အထူးသဖြင့် အရေးကြီးတဲ့ Server တွေမှာ သုံးပါတယ်။ မှတ်တမ်းတစ်ခုမှ အဖျက်မခံနိုင်တဲ့ အခြေအနေမျိုးအတွက် ဖြစ်ပါတယ်။
* **သတိထားရန်:** Disk space ပြည့်မသွားအောင်တော့ အမြဲစစ်ပေးရပါမယ်။


3. **Do not overwrite events (Clear logs manually):**
* **အလုပ်လုပ်ပုံ:** Log ပြည့်သွားရင် မှတ်တမ်းအသစ် ထပ်မရေးတော့ပါဘူး။ User က manual လာဖျက်မှ ထပ်ရေးပါတယ်။
* **CIS သတိပေးချက်:** **ဒါကို မသုံးသင့်ပါ။** Log ပြည့်သွားတဲ့အချိန်နဲ့ ကျနော်တို့ သိလိုက်တဲ့အချိန်ကြားက တိုက်ခိုက်မှုတွေကို လုံးဝ မှတ်တမ်းတင်နိုင်တော့မှာ မဟုတ်လို့ပါ။

---

## **၃။ CIS Standard Summary**

| Setting | CIS Recommended Value | Security Impact |
| --- | --- | --- |
| **Max Log Size** | 1,024,000 KB (1 GB) | Prevents log data loss |
| **Retention Method** | Archive when full | Ensures forensic history |
| **Monitoring** | Event ID 1, 3, 22 (DNS) | Full visibility of attacks |

---

### Professional Note:**

> "Sysmon ကို Setup လုပ်တဲ့အခါ **'Archive the log when full'** ကို သုံးခြင်းက Compliance အရ အကောင်းဆုံး ဖြစ်ပေမဲ့ Disk space management ကိုပါ ထည့်သွင်းစဉ်းစားရပါမယ်။ အကယ်၍ Disk မလောက်ငှပါက **'Overwrite events'** ကို သုံးပြီး Log size ကို အမြင့်ဆုံး (ဥပမာ 2GB) အထိ တိုးမြှင့်ထားခြင်းဖြင့် မျှခြေညှိနိုင်ပါတယ်"
