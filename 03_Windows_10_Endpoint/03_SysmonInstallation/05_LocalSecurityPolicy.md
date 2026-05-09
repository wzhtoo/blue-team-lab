# Local Security Policy
 ထဲက **Advanced Audit Policy Configuration** ဆိုတာ ပုံမှန် Audit Policy ထက် ပိုပြီး "Micro" ကျကျ (အသေးစိတ်) ထိန်းချုပ်နိုင်တဲ့ နေရာဖြစ်ပါတယ်။

CIS Benchmark မှာလည်း ပုံမှန် Audit Policy ထက်စာရင် ဒီ **Advanced Audit Policy** ကို ပိုပြီး အသုံးပြုဖို့ အကြံပြုထားပါတယ်။ ဘာကြောင့်လဲဆိုတော့ မလိုအပ်တဲ့ Log တွေအများကြီးတက်မလာအောင် Filter လုပ်ပြီးသား ဖြစ်စေလို့ပါ။

လမ်းကြောင်း- `secpol.msc` > **Advanced Audit Policy Configuration** > **System Audit Policies - Local Group Policy Object**
---

## **Advanced Audit Policy (အဓိက Category များ)**

ဒီအောက်မှာ Category ပေါင်း ၁၀ ခုရှိပေမဲ့ CIS က အလေးပေးတဲ့ အရေးကြီးဆုံးအချက်တွေကို အသေးစိတ် ရှင်းပြပေးပါ့မယ်။

### **၁။ Account Logon**

* **Audit Credential Validation:** User တစ်ယောက်ရဲ့ Password အစစ်အမှန် ဟုတ်/မဟုတ် စစ်ဆေးမှုကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Recommendation:** `Success & Failure`
* **Security Value:** တိုက်ခိုက်သူက Password တွေကို ခန့်မှန်းရိုက်နေတာ (Brute Force) ကို သိရှိနိုင်ဖို့ပါ။

### **၂။ Account Management**

* **Audit User Account Management:** User အသစ်ဆောက်တာ၊ နာမည်ပြောင်းတာ၊ Password reset ချတာတွေကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Recommendation:** `Success`
* **Security Value:** ခွင့်ပြုချက်မရှိဘဲ အကောင့်အသစ်တွေ ဖန်တီးပြီး နောက်ဖေးပေါက် (Backdoor) လုပ်တာကို တားဆီးဖို့ပါ။

### **၃။ Detailed Tracking**

* **Audit Process Creation:** Program တစ်ခု စဖွင့်လိုက်တိုင်း မှတ်တမ်းတင်ပါတယ်။ (ဒါဟာ Sysmon Event ID 1 နဲ့ အတူတူပါပဲ)။
* **CIS Recommendation:** `Success`
* **Security Value:** Malware တွေက Background မှာ ဘာတွေ Run သွားလဲဆိုတာ ခြေရာခံဖို့ပါ။

### **၄။ Logon/Logoff**

* **Audit Logon:** User တစ်ယောက် စက်ထဲဝင်လာတာကို မှတ်တမ်းတင်ပါတယ်။
* **Audit Logoff:** စက်ကနေ ပြန်ထွက်သွားတာကို မှတ်တမ်းတင်ပါတယ်။
* **Audit Special Logon:** Admin အခွင့်အရေးရှိတဲ့သူ ဝင်လာရင် အထူးမှတ်တမ်းတင်ပါတယ်။
* **CIS Recommendation:** `Success & Failure`

### **၅။ Object Access**

* **Audit File System:** File/Folder တွေကို ဝင်ကြည့်တာ၊ ဖျက်တာတွေကို မှတ်တမ်းတင်ပါတယ်။
* **Audit Removable Storage:** USB Stick တွေ ထိုးတာ၊ အထဲကဒေတာတွေ ကူးတာကို မှတ်တမ်းတင်ပါတယ်။
* **CIS Recommendation:** `Success & Failure` (လိုအပ်တဲ့ Folder တွေမှာ Audit သီးသန့်ပေးထားမှသာ ပေါ်မှာဖြစ်ပါတယ်)။

---

## **CIS Standard နဲ့အညီ ညှိနှိုင်းရမယ့် အရေးကြီးဆုံးအချက် (Pro Tip)**

အရေးကြီးဆုံးသတိပေးချက် တစ်ခုရှိပါတယ်။ **Advanced Audit Policy** ကို သုံးမယ်ဆိုရင် ပုံမှန် **Basic Audit Policy** နဲ့ မရောထွေးသွားအောင် "Force" လုပ်ပေးရတဲ့ setting တစ်ခု ရှိပါတယ်။

**Security Options** ထဲက:

> `Audit: Force audit policy subcategory settings to override audit policy category settings` ကို **Enabled** လုပ်ထားရပါမယ်။

**ဘာကြောင့်လဲ:** ဒါကို Enable မလုပ်ထားရင် Windows က အသေးစိတ်ပြင်ထားတဲ့ Advanced settings တွေကို လျစ်လျူရှုပြီး ပုံမှန် Basic settings တွေကိုပဲ သုံးနေမှာ ဖြစ်လို့ပါ။

---

## **Advanced Audit Policy**

| Subcategory | CIS Recommendation | Target Action |
| --- | --- | --- |
| **Credential Validation** | Success & Failure | NTLM/Kerberos monitoring |
| **Process Creation** | Success | Malware tracking |
| **User Account Mgmt** | Success | Unauthorized account creation |
| **Special Logon** | Success | Admin activity tracking |
| **File System** | Success & Failure | Data theft monitoring |

---

### **(Practical Guide)**

> "Advanced Audit Policy သည် ပုံမှန် Audit Policy ထက် ပိုမိုတိကျသော Event ID များကို ထုတ်ပေးနိုင်သဖြင့် Blue Team တစ်ယောက်အတွက် ပိုမိုအသုံးဝင်သည်။ သို့သော် Log များလွန်းသဖြင့် Disk Space ပြည့်သွားနိုင်သောကြောင့် အရေးကြီးသော Subcategories များကိုသာ ရွေးချယ် Audit လုပ်သင့်သည် (Selective Auditing)။"
