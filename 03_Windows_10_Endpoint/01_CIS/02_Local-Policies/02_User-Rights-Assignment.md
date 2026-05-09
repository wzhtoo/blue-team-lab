# User Rights Assignment

## **Local Policies: User Rights Assignment (အသေးစိတ် ရှင်းလင်းချက်)**

**User Rights Assignment** ဆိုတာကတော့ ကွန်ပျူတာ System တစ်ခုပေါ်မှာရှိတဲ့ User တွေ သို့မဟုတ် Group တွေက ဘယ်လို **"လုပ်ပိုင်ခွင့်"** တွေ ရှိသလဲဆိုတာကို သတ်မှတ်တာ ဖြစ်ပါတယ်။ ဒါဟာ ဖိုင်တွေကို Permission ပေးတာမျိုး မဟုတ်ဘဲ System ရဲ့ ပင်မလုပ်ဆောင်ချက်တွေ (ဥပမာ - စက်ပိတ်ခြင်း၊ အချိန်ပြောင်းခြင်း) ကို ထိန်းချုပ်တာ ဖြစ်ပါတယ်။

လမ်းကြောင်း: `gpedit.msc` > `Computer Configuration` > `Windows Settings` > `Security Settings` > `Local Policies` > `User Rights Assignment`

---

### **၁။ Allow log on locally (စက်ရှေ့မှာတင် Login ဝင်ခွင့်)**

* **လုပ်ဆောင်ချက်:** ဒီကွန်ပျူတာရဲ့ Keyboard နဲ့ Mouse ကိုသုံးပြီး ဘယ်သူတွေ Login ဝင်ခွင့်ရှိမလဲဆိုတာကို သတ်မှတ်ပါတယ်။
* **CIS Standard:** `Administrators`, `Users` (Server ဖြစ်ပါက `Administrators` သာ)။
* **ရည်ရွယ်ချက်:** ခွင့်ပြုချက်မရှိသူတွေ စက်ရှေ့မှာတင် အကောင့်ထဲဝင်ပြီး ဒေတာတွေခိုးယူတာမျိုး မလုပ်နိုင်အောင် ကန့်သတ်ခြင်း ဖြစ်ပါတယ်။

### **၂။ Allow log on through Remote Desktop Services**

* **လုပ်ဆောင်ချက်:** Network ပေါ်ကနေ Remote Desktop (RDP) သုံးပြီး ဘယ်သူတွေ လှမ်းဝင်ခွင့်ရှိမလဲဆိုတာပါ။
* **CIS Standard:** `Administrators`, `Remote Desktop Users` (လိုအပ်မှသာ သတ်မှတ်ရန်)။
* **ရည်ရွယ်ချက်:** RDP ဟာ ဟက်ကာတွေ အကြိုက်ဆုံးလမ်းကြောင်း ဖြစ်လို့ တကယ်လိုအပ်တဲ့သူကိုပဲ ခွင့်ပြုရပါမယ်။

### **၃။ Change the system time (စက်၏ အချိန်ကို ပြောင်းလဲခြင်း)**

* **လုပ်ဆောင်ချက်:** System clock ကို ပြောင်းလဲခွင့် ဖြစ်ပါတယ်။
* **CIS Standard:** `Administrators`, `LOCAL SERVICE`
* **ရည်ရွယ်ချက်:** Cyber Security မှာ **Log entries** တွေရဲ့ အချိန်ဟာ အရမ်းအရေးကြီးပါတယ်။ ဟက်ကာက အချိန်ကို ပြောင်းလိုက်ရင် သူဘယ်ချိန်က ဝင်သွားလဲဆိုတာကို ခြေရာခံရ ခက်သွားစေနိုင်ပါတယ်။

### **၄။ Force shutdown from a remote system**

* **လုပ်ဆောင်ချက်:** Network ပေါ်ကနေ ဒီစက်ကို အဝေးကနေ ပိတ်ခိုင်းတာ (Shutdown) ဖြစ်ပါတယ်။
* **CIS Standard:** `Administrators`
* **ရည်ရွယ်ချက်:** သာမန် User တွေ ဒါမှမဟုတ် တိုက်ခိုက်သူတွေက Server ကို အဝေးကနေ ပိတ်ပစ်လိုက်ခြင်းဖြင့် **Denial of Service (DoS)** မဖြစ်အောင် ကာကွယ်ခြင်း ဖြစ်ပါတယ်။

### **၅။ Log on as a service**

* **လုပ်ဆောင်ချက်:** အကောင့်တစ်ခုကနေ Background မှာ Service တစ်ခုအနေနဲ့ အလုပ်လုပ်ခွင့် ရှိ၊ မရှိ သတ်မှတ်တာပါ။
* **CIS Standard:** `No users` (လိုအပ်တဲ့ Service Accounts များကိုသာ သီးသန့်ထည့်ရန်)။
* **ရည်ရွယ်ချက်:** Malware တွေက သူတို့ကိုယ်သူတို့ Service အနေနဲ့ မှတ်ပုံတင်ပြီး အမြဲ run နေအောင် ကြိုးစားတတ်လို့ ဖြစ်ပါတယ်။

### **၆။ Manage auditing and security log**

* **လုပ်ဆောင်ချက်:** Security Log တွေကို ကြည့်ရှုခွင့်နဲ့ ဖျက်ပစ်ခွင့် ဖြစ်ပါတယ်။
* **CIS Standard:** `Administrators`
* **ရည်ရွယ်ချက်:** ဒါဟာ အရမ်းအရေးကြီးပါတယ်။ တိုက်ခိုက်သူက သူလုပ်ထားတဲ့ သက်သေတွေကို ဖျက်ချင်ရင် ဒီအခွင့်အရေး လိုအပ်ပါတယ်။ ဒါကြောင့် အယုံကြည်ရဆုံး Admin ကိုပဲ ပေးရပါမယ်။

### **၇။ Shut down the system**

* **လုပ်ဆောင်ချက်:** စက်ကို Shutdown ချခွင့် ဖြစ်ပါတယ်။
* **CIS Standard:** `Administrators`, `Users`
* **ရည်ရွယ်ချက်:** Server တွေမှာဆိုရင်တော့ `Administrators` သာ ပေးလေ့ရှိပါတယ်။ သာမန် User က မှားပြီး ပိတ်လိုက်ရင် ဝန်ဆောင်မှုတွေ ရပ်ဆိုင်းကုန်မှာ စိုးလို့ ဖြစ်ပါတယ်။
---

### **User Rights Assignment Summary (Checklist for Repo)**

| Right / Policy | Recommended Users (CIS) | Security Goal |
| --- | --- | --- |
| Allow log on locally | Admin, Users | Physical Access Control |
| Change the system time | Admin, Local Service | Log Integrity |
| Force shutdown from remote | Admin | Prevent DoS Attacks |
| Manage auditing and security log | Admin | Prevent Anti-forensics |
| Shut down the system | Admin, Users | Availability |

---

### မှတ်ချက်

> "User Rights Assignment ကို သတ်မှတ်တဲ့အခါ **'Principle of Least Privilege' (PoLP)** ကို သုံးရပါမယ်။ ဆိုလိုတာက လူတစ်ယောက်ကို သူလုပ်ရမယ့် အလုပ်အတွက် လိုအပ်တဲ့ အနိမ့်ဆုံး အခွင့်အရေးကိုပဲ ပေးတာ ဖြစ်ပါတယ်။ အပိုအခွင့်အရေးတွေ ပေးထားမိရင် တိုက်ခိုက်သူက အဲဒီအကောင့်ကို ရသွားတဲ့အခါ System တစ်ခုလုံးကို ထိန်းချုပ်သွားနိုင်လို့ ဖြစ်ပါတယ်။"
---
