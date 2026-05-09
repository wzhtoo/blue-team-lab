# Audit Policy

## **Local Policies: Audit Policy (အသေးစိတ် ရှင်းလင်းချက်)**

**Audit Policy** ဆိုတာ System အတွင်းမှာ ဖြစ်ပျက်သမျှ လုပ်ဆောင်ချက်တွေကို မှတ်တမ်းတင်တဲ့ (Logging) စနစ်ဖြစ်ပါတယ်။ Cyber Security (Blue Team) ရှုထောင့်ကကြည့်ရင် တိုက်ခိုက်သူက ဘယ်အချိန်မှာ ဘာလုပ်သွားလဲဆိုတာကို **Windows Event Viewer** ထဲမှာ သက်သေအဖြစ် ပြန်ရှာနိုင်ဖို့ ဒီ Policy တွေကို ဖွင့်ထားရခြင်း ဖြစ်ပါတယ်။

လမ်းကြောင်း: `gpedit.msc` > `Computer Configuration` > `Windows Settings` > `Security Settings` > `Local Policies` > `Audit Policy`

---

### **၁။ Audit account logon events**

* **လုပ်ဆောင်ချက်:** User တစ်ယောက်က ဒီကွန်ပျူတာကို သုံးပြီး (Network ပေါ်ကနေဖြစ်စေ၊ ကိုယ်တိုင်ဖြစ်စေ) အကောင့်ဝင်ဖို့ ကြိုးစားတာကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** `Failure` ကို ဖွင့်ထားခြင်းဖြင့် တစ်စုံတစ်ယောက်က Password တွေကို တရစပ် မှန်းရိုက်နေတာ (Brute-force Attack) ကို သိနိုင်ပါတယ်။ `Success` ကတော့ ဘယ်အချိန်မှာ ဘယ်သူဝင်သွားလဲဆိုတာကို သိနိုင်ဖို့ပါ။

### **၂။ Audit account management**

* **လုပ်ဆောင်ချက်:** User အသစ်ဆောက်တာ၊ Password ပြောင်းတာ၊ အကောင့်ကို ဖျက်ပစ်တာ ဒါမှမဟုတ် Group ထဲကို လူအသစ်ထည့်တာ စတဲ့ အပြောင်းအလဲတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** တိုက်ခိုက်သူတွေဟာ System ထဲရောက်တာနဲ့ နောက်တစ်ခါ အလွယ်တကူ ပြန်ဝင်လို့ရအောင် (Persistence) အကောင့်အသစ်တွေ ဆောက်လေ့ရှိပါတယ်။ ဒါကို သိဖို့အတွက် ဖြစ်ပါတယ်။

### **၃။ Audit directory service access**

* **လုပ်ဆောင်ချက်:** Active Directory (AD) ထဲက Object တွေကို ဝင်ရောက်ကြည့်ရှုတာကို မှတ်တမ်းတင်ပါတယ်။ (ဒါက Domain Controller တွေမှာ ပိုအရေးကြီးပါတယ်)။
* **CIS Standard:** `No Auditing` (Standard PC များအတွက်) သို့မဟုတ် လိုအပ်မှဖွင့်ရန်။

### **၄။ Audit logon events**

* **လုပ်ဆောင်ချက်:** အပေါ်က "Account logon events" နဲ့ ဆင်တူသော်လည်း ဒါက logon session တစ်ခုလုံးကို ကြည့်တာပါ။ (ဥပမာ - Service တစ်ခုက အလုပ်လုပ်ဖို့ logon ဝင်တာမျိုး)။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** Session တည်ဆောက်ပုံတွေကို ခြေရာခံဖို့ပါ။

### **၅။ Audit object access**

* **လုပ်ဆောင်ချက်:** File တွေ၊ Folder တွေ၊ Registry Key တွေကို ဖွင့်ကြည့်တာ၊ ပြင်တာတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure` (မှတ်ချက်- ဒါကိုဖွင့်ရုံနဲ့ မပြီးသေးဘဲ သက်ဆိုင်ရာ File/Folder ရဲ့ Properties > Security > Advanced > Auditing မှာ ဘယ်သူ့ကို စောင့်ကြည့်မှာလဲဆိုတာ သွားသတ်မှတ်ပေးရပါသေးတယ်)။
* **ဘာကြောင့် ဖွင့်ရသလဲ:** အရေးကြီးတဲ့ Confidential data တွေကို ဘယ်သူခိုးကြည့်လဲဆိုတာ သိဖို့ပါ။

### **၆။ Audit policy change**

* **လုပ်ဆောင်ချက်:** အခုကျနော်တို့ ပြင်နေတဲ့ Security Policy တွေကို တစ်စုံတစ်ယောက်က လာပြင်ရင် မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** ဟက်ကာတွေက သူတို့လုပ်တာတွေကို ဖုံးကွယ်ဖို့ Logging/Audit Policy တွေကို လာပိတ်တတ်ပါတယ်။ အဲဒီလို လာပိတ်တာကို သိနိုင်ဖို့ ဖြစ်ပါတယ်။

### **၇။ Audit privilege use**

* **လုပ်ဆောင်ချက်:** User တစ်ယောက်က သူ့ရဲ့ အထူးအခွင့်အရေး (ဥပမာ - Admin rights) ကို သုံးပြီး System clock ပြောင်းတာ၊ ဖိုင်တွေကို backup ယူတာမျိုးကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** အခွင့်အရေးကို အလွဲသုံးစားလုပ်တာ (Privilege Escalation) ရှိမရှိ စစ်ဆေးဖို့ပါ။

### **၈။ Audit process tracking**

* **လုပ်ဆောင်ချက်:** Application တစ်ခု ဖွင့်လိုက်တာ (Process start)၊ ပိတ်လိုက်တာ (Process exit) စတာတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `No Auditing` (သို့သော် Advanced Auditing တွင် Success ကို ဖွင့်ရန် အကြံပြုလေ့ရှိသည်)။
* **ဘာကြောင့် ဖွင့်ရသလဲ:** Malware တွေက ဘယ်အချိန်မှာ run သွားလဲဆိုတာကို `cmd.exe` သို့မဟုတ် `powershell.exe` run တဲ့ မှတ်တမ်းကို ကြည့်ပြီး သိနိုင်ပါတယ်။

### **၉။ Audit system events**

* **လုပ်ဆောင်ချက်:** ကွန်ပျူတာ Shutdown ချတာ၊ Restart ချတာ၊ Security Log တွေကို ဖျက်လိုက်တာ (Clear Logs) တွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Standard:** `Success, Failure`
* **ဘာကြောင့် ဖွင့်ရသလဲ:** တိုက်ခိုက်သူတွေဟာ သက်သေဖျောက်ဖို့ Log တွေကို ဖျက်လေ့ရှိပါတယ်။ "Log clear" ဖြစ်သွားတဲ့ မှတ်တမ်းက အရမ်းအရေးကြီးပါတယ်။

---

### **Audit Policy Summary (GitHub Repo အတွက် Checklist)**

| Policy | Setting (CIS Standard) | Goal |
| --- | --- | --- |
| Account Logon | Success, Failure | Detect Brute-force |
| Account Management | Success, Failure | Detect New User Creation |
| Logon Events | Success, Failure | Session Monitoring |
| Object Access | Success, Failure | File Integrity Monitoring |
| Policy Change | Success, Failure | Anti-tampering |
| Privilege Use | Success, Failure | Detect Admin Misuse |
| System Events | Success, Failure | Detect Log Clearing |
---

# Audit Policy

Audit Policy မှာ **Success** ရော **Failure** ရော နှစ်ခုလုံးကို ဖွင့်ထားဖို့ (သတ်မှတ်ဖို့) အကြံပြုပါတယ်ခင်ဗျာ။ ဒါကို **Dual Auditing** လို့ ခေါ်ပါတယ်။

ဘာကြောင့် နှစ်ခုလုံး လိုအပ်တာလဲဆိုတာကို အလွယ်ကူဆုံး ရှင်းပြပေးပါ့မယ်။
---

### ၁။ Failure (ကျရှုံးမှု) ကို ဘာကြောင့် သတ်မှတ်ရသလဲ။

**"တိုက်ခိုက်မှုကို ကြိုတင်သိရှိရန်"** ဖြစ်ပါတယ်။

* **ဥပမာ:** တစ်စုံတစ်ယောက်က သင့်အကောင့်ကို Password မှန်းပြီး ၁ မိနစ်အတွင်း အကြိမ် ၅၀ ရိုက်နေတယ်ဆိုပါစို့။ အဲဒါတွေအားလုံးက **Failure** တွေဖြစ်မှာပါ။
* ဒီမှတ်တမ်းကို ကြည့်ပြီး "ဪ... ငါ့ကို တစ်ယောက်ယောက်က Brute-force လုပ်နေပြီပဲ" ဆိုတာကို သိနိုင်ပြီး အချိန်မီ ကာကွယ်နိုင်ပါတယ်။

### ၂။ Success (အောင်မြင်မှု) ကို ဘာကြောင့် သတ်မှတ်ရသလဲ။

**"တိုက်ခိုက်သူ ဝင်သွားပြီးနောက် ဘာတွေလုပ်သွားလဲ သိရန်"** ဖြစ်ပါတယ်။

* **ဥပမာ:** ဟက်ကာက Password ကို မှန်းရိုက်ရင်းနဲ့ နောက်ဆုံးမှာ မှန်သွားပြီး Login အောင်မြင်သွားတယ်ဆိုပါစို့ (**Success** ဖြစ်သွားပြီ)။
* ဒီ Success မှတ်တမ်းကို ကြည့်မှသာ "ဟက်ကာက ဘယ်အချိန်မှာ ငါ့စက်ထဲကို အောင်မြင်စွာ ဝင်သွားတာလဲ၊ ဝင်ပြီးတော့ ဘယ်ဖိုင်တွေကို ဖွင့်ကြည့်သွားတာလဲ" ဆိုတာကို ပြန်ခြေရာခံလို့ ရမှာပါ။

---

### CIS Standard အရ ဘယ်ဟာကို ရွေးရမလဲ?

CIS Benchmark ကတော့ အရေးကြီးတဲ့ Policy တော်တော်များများမှာ **Success နှင့် Failure နှစ်ခုလုံး** ကို အမှန်ခြစ်ပေးဖို့ (Enable လုပ်ဖို့) တိုက်တွန်းထားပါတယ်။

| Policy အမျိုးအစား | Success | Failure | အကြောင်းရင်း |
| --- | --- | --- | --- |
| **Account Logon** | ✅ | ✅ | ဘယ်သူဝင်လဲသိဖို့နဲ့ ခိုးဝင်တာကို သိဖို့ |
| **Account Management** | ✅ | ✅ | အကောင့်အသစ်ဆောက်တာနဲ့ password ပြောင်းတာသိဖို့ |
| **Object Access** | ✅ | ✅ | ဖိုင်တွေကို ဘယ်သူဖတ်လဲနဲ့ ဘယ်သူခိုးဖွင့်လဲသိဖို့ |
| **Policy Change** | ✅ | ✅ | လုံခြုံရေး setting တွေကို လာပြင်တာ သိဖို့ |
| **System Events** | ✅ | ✅ | စက်ပိတ်တာနဲ့ Log တွေ ဖျက်တာကို သိဖို့ |

---


### မှတ်ချက်

> "Audit Policy သတ်မှတ်တဲ့အခါ **Success** ကိုပဲ သတ်မှတ်ရင် ကျရှုံးတဲ့ တိုက်ခိုက်မှုတွေကို မသိနိုင်သလို၊ **Failure** ကိုပဲ သတ်မှတ်ရင်လည်း အောင်မြင်သွားတဲ့ တိုက်ခိုက်မှုတွေကို ခြေရာခံလို့ မရနိုင်ပါဘူး။ ဒါကြောင့် အပြည့်အစုံ သိနိုင်ဖို့ **Success & Failure** နှစ်ခုလုံးကို Audit လုပ်သင့်ပါတယ်"

