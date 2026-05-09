# Security Options 

ဒီအပိုင်းမှာ Policy ပေါင်း ရာနဲ့ချီရှိလို့ CIS က အလေးပေးတဲ့ **"Critical Settings"** တွေကိုပဲ အဓိကထားပြီး ရှင်းပြပါ့မယ်။

လမ်းကြောင်း: `gpedit.msc` > `Computer Configuration` > `Windows Settings` > `Security Settings` > `Local Policies` > `Security Options`
---

## **Local Policies: Security Options (အသေးစိတ် ရှင်းလင်းချက်)**

Security Options ဆိုတာ Windows ရဲ့ အပြုအမူ (Behavior) တွေကို ထိန်းချုပ်တာပါ။ ဥပမာ - User နာမည်ကို ဘယ်လိုပြမလဲ၊ အကောင့်တွေကို ဘယ်လိုစီမံမလဲ စတာတွေ ပါဝင်ပါတယ်။

### **၁။ အကောင့်ဆိုင်ရာ မူဝါဒများ (Accounts Group)**

* **Accounts: Administrator account status**
* **CIS Standard:** `Disabled`
* **ဘာကြောင့်လဲ:** Built-in Admin အကောင့်ဟာ Password မရှိဘဲ ပွင့်နေတတ်သလို၊ တိုက်ခိုက်သူတွေရဲ့ နံပါတ် (၁) ပစ်မှတ် ဖြစ်လို့ပါ။


* **Accounts: Rename administrator/guest account**
* **CIS Standard:** `Enabled (နာမည်အသစ်ပေးရန်)`
* **ဘာကြောင့်လဲ:** နာမည်ကို `Admin-local-01` စသဖြင့် ပြောင်းထားရင် တိုက်ခိုက်သူက User name ကိုပါ လိုက်မှန်းနေရလို့ အချိန်ပိုကြာသွားစေပါတယ်။


### **၂။ ကွန်ရက်ဝင်ရောက်မှုဆိုင်ရာ (Network Access Group)**

* **Network access: Allow anonymous SID/Name translation**
* **CIS Standard:** `Disabled`
* **ဘာကြောင့်လဲ:** ဒါကို ဖွင့်ထားရင် ဟက်ကာက ကွန်ယက်ထဲကနေ User တွေရဲ့ နာမည်တွေကို အလွယ်တကူ ဆွဲထုတ် (Enumerate) လို့ ရသွားနိုင်ပါတယ်။


* **Network access: Do not allow anonymous enumeration of SAM accounts/shares**
* **CIS Standard:** `Enabled`
* **ဘာကြောင့်လဲ:** ဘယ်သူမှန်းမသိတဲ့ (Anonymous) User တွေကို အကောင့်စာရင်းနဲ့ Share folder စာရင်းတွေကို မမြင်ရအောင် ပိတ်ပင်တာ ဖြစ်ပါတယ်။


### **၃။ Login ဝင်ရောက်မှုဆိုင်ရာ (Interactive Logon Group)**

* **Interactive logon: Do not display last user name**
* **CIS Standard:** `Enabled`
* **ဘာကြောင့်လဲ:** Login screen မှာ နောက်ဆုံးဝင်ထားတဲ့ User name ကို ပြမထားရင် တိုက်ခိုက်သူက Username ရော Password ရော နှစ်ခုလုံးကို မှန်းရိုက်ရမှာဖြစ်လို့ ပိုလုံခြုံပါတယ်။


* **Interactive logon: Message text/title for users attempting to log on**
* **CIS Standard:** `Legal Notice စာသားထည့်ရန်`
* **ဘာကြောင့်လဲ:** "ခွင့်ပြုချက်မရှိဘဲ ဝင်ရောက်ခြင်းကို ဥပဒေအရ အရေးယူမည်" ဆိုတဲ့စာသားမျိုး ပြထားခြင်းဟာ ဥပဒေကြောင်းအရ (Compliance) အရမ်းအရေးကြီးပါတယ်။


### **၄။ Microsoft Network Server/Client အပိုင်း**

* **Microsoft network server: Digitally sign communications (always)**
* **CIS Standard:** `Enabled`
* **ဘာကြောင့်လဲ:** Network ပေါ်မှာ ဒေတာတွေ ပို့တဲ့အခါ (ဥပမာ - SMB) အလယ်ကနေ ကြားဖြတ်နှောင့်ယှက်တာ (Man-in-the-Middle Attack) ကို ကာကွယ်ဖို့အတွက် Digital signature သုံးတာ ဖြစ်ပါတယ်။



### **၅။ User Account Control (UAC) အပိုင်း**

(ဒါကတော့ Security Options ရဲ့ အောက်ဆုံးမှာ ရှိပါတယ်)

* **Run all administrators in Admin Approval Mode:** `Enabled`
* **Behavior of the elevation prompt for administrators:** `Prompt for consent on the secure desktop`

---

### **Security Options Summary (Repo Checklist)**

| Policy Group | Setting (CIS) | Security Purpose |
| --- | --- | --- |
| **Accounts** | Rename Admin/Guest | Obscurity (ခန့်မှန်းရခက်အောင်လုပ်ခြင်း) |
| **Network Access** | Restrict Anonymous | Prevent Enumeration (အချက်အလက်ခိုးယူခြင်းတားဆီးခြင်း) |
| **Interactive Logon** | Don't display username | Privacy & Security |
| **UAC** | Admin Approval Mode | Anti-Malware / Privilege Protection |

---

### ဖြည့်စွက်ချက်:**

> "Security Options တွေကို ပြင်ဆင်တဲ့အခါ အချို့သော Network Settings တွေ (ဥပမာ - SMB Signing) က အဟောင်းတွေနဲ့ မကိုက်ညီတာမျိုး (Compatibility Issue) ရှိတတ်ပါတယ်။ ဒါကြောင့် မပြင်ခင်မှာ လုပ်ငန်းခွင်က Software တွေနဲ့ အဆင်ပြေ၊ မပြေကို စစ်ဆေးဖို့ အရေးကြီးပါတယ်။ လုံခြုံရေးဆိုတာ Balance (မျှခြေ) ညှိရတဲ့ အလုပ်မျိုးပါ"

---