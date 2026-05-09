# User Rights Assignment

policy ပေါင်းများစွာ ရှိပါတယ်။ အဲဒါတွေကို အကုန်လုံး လိုက်ပြင်နေဖို့ဆိုတာ လက်တွေ့မှာ မဖြစ်နိုင်သလို၊ တချို့ settings တွေက မှားပြင်မိရင် System ကိုပါ Error တက်စေနိုင်ပါတယ်။

CIS (Center for Internet Security) Benchmark ထဲမှာ **အဲဒီအချက်တွေ အကုန်ပါပါတယ်**။ ဒါပေမဲ့ သူတို့က အရေးကြီးပုံချင်း မတူကြပါဘူး။
---

### ၁။ အရေးကြီးဆုံး CIS Policies (သိထားသင့်တဲ့အချက်များ)

ဒါတွေက တိုက်ခိုက်သူတွေ အသုံးချလေ့ရှိတဲ့ အပေါက်တွေမို့ CIS က သေချာ သတ်မှတ်ခိုင်းတာပါ။

* **Allow log on locally / RDP:** (စောစောက ရှင်းပြခဲ့တဲ့အချက်)
* **Debug programs:** * **CIS:** `Administrators` သာ။
* **ရှင်းလင်းချက်:** ဒါက အရမ်းအန္တရာယ်များပါတယ်။ တိုက်ခိုက်သူက ဒီအခွင့်အရေးရရင် OS ရဲ့ Memory ထဲက Password တွေကို ဆွဲထုတ်လို့ ရသွားနိုင်ပါတယ်။


* **Take ownership of files or other objects:** * **CIS:** `Administrators` သာ။
* **ရှင်းလင်းချက်:** ဖိုင်တစ်ခုကို ပိုင်ရှင်ပြောင်းပြီး ခိုးဖတ်လို့ရစေတဲ့ အခွင့်အရေးပါ။


### ၂။ Technical အချက်များ (Process & Service)

`Debug programs`, `Increase a process working set`, `Synchronize directory service data` စတာတွေက **Windows ရဲ့ အတွင်းပိုင်း အလုပ်လုပ်ပုံ (System Internals)** တွေပါ။

* **Increase a process working set:** ဒါက program တစ်ခုက memory ဘယ်လောက်သုံးမလဲဆိုတာပါ။ CIS မှာ ဒါကို `Local Service` တို့ `Standard Users` တို့ ပေးထားလေ့ရှိပါတယ်။ (ဒါကို ပိတ်လိုက်ရင် software အချို့ run လို့ မရတော့ပါဘူး)။
* **Synchronize directory service data:** ဒါက Domain Controller (Server) တွေမှာပဲ အဓိက သုံးတာပါ။ သာမန် PC တွေမှာတော့ ဘယ်သူ့ကိုမှ ပေးစရာ မလိုပါဘူး။

### ၃။ ဘာလို့ အကုန်လုံးကို အသေးစိတ် မပြင်ကြတာလဲ?

အကုန်လုံးကို လိုက်ပြင်ဖို့ အဆင်မပြေရတဲ့ အကြောင်းရင်း ၃ ချက် ရှိပါတယ်-

1. **Default Settings are usually Okay:** Windows ရဲ့ default settings တော်တော်များများက (ဥပမာ - `Increase a process working set`) သာမန်အားဖြင့် လုံခြုံပြီးသားပါ။
2. **Break Functions:** `Debug programs` လိုမျိုးကို သေချာမသိဘဲ လျှော့ချလိုက်ရင် တစ်ချို့သော Application တွေ အလုပ်မလုပ်တော့တာမျိုး ကြုံရနိုင်ပါတယ်။
3. **Complexity:** လူတစ်ယောက်က policy ၄၀ စလုံးကို အလွတ်မှတ်ထားဖို့ မဖြစ်နိုင်ပါဘူး။
---

### The Practical Way

တစ်ခုချင်း ရေးမယ့်အစား အောက်ပါအတိုင်း **"အုပ်စုခွဲပြီး"** ရေးသားတာက ပိုတန်ဖိုးရှိပါတယ်-

1. **High Priority (Must Set):** အပေါ်မှာ ရှင်းပြခဲ့တဲ့ ၇ ချက် + `Debug Programs`။ (ဒါတွေက လုံခြုံရေးအတွက် အဓိက တံခါးပေါက်တွေပါ)။
2. **Advanced/Service Rights:** `Log on as a batch job`, `Log on as a service`။ (ဒါတွေက Server Admin တွေအတွက်ပါ)။
3. **Default/System Rights:** `Increase scheduling priority`, `Increase a process working set`။ (ဒါတွေကိုတော့ Windows ရဲ့ Default အတိုင်းပဲ ထားသင့်ပါတယ်)။

**အနှစ်ချုပ်:**
CIS မှာ အကုန်ပါပေမယ့် Cybersecurity Professional တစ်ယောက်အနေနဲ့ **"တိုက်ခိုက်သူ အသုံးချနိုင်တဲ့ အပေါက် (Attack Surface)"** ဖြစ်တဲ့ Policy တွေကိုပဲ အဓိက အာရုံစိုက် ပြင်ဆင်လေ့ ရှိပါတယ်။ 

အမှန်တော့ Cyber Security (Blue Team) သမားတစ်ယောက်ရဲ့ အဓိကကျွမ်းကျင်မှုက **"ဘယ်အရာကို မထိဘဲထားရမလဲ"** နဲ့ **"ဘယ်အရာကို အသေအချာ Hardening လုပ်ရမလဲ"** ဆိုတာကို ခွဲခြားသိမြင်တာပါပဲ။