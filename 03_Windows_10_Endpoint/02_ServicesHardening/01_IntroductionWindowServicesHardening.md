## **Introduction to Windows Services Hardening**
Blue Team (Cyber Defense) ရဲ့ အရေးအကြီးဆုံး အစိတ်အပိုင်းတစ်ခုပါ။

### **Windows Services ဆိုတာဘာလဲ?**

Windows Services ဆိုတာ အသုံးပြုသူ (User) က စက်ကို Login မဝင်ခင်ကတည်းက နောက်ကွယ် (Background) မှာ အမြဲတမ်း run နေတဲ့ Program တွေဖြစ်ပါတယ်။ ဥပမာ - Network ချိတ်ဆက်မှု၊ Print ထုတ်ခြင်း၊ Update စစ်ဆေးခြင်း စတာတွေကို ဒီ Services တွေက လုပ်ဆောင်ပေးပါတယ်။

### **Services Hardening ဆိုတာ ဘာလဲ?**

**Services Hardening** ဆိုတာ ကွန်ပျူတာရဲ့ တိုက်ခိုက်ခံရနိုင်ခြေ (Attack Surface) ကို လျှော့ချဖို့အတွက် မလိုအပ်တဲ့ Services တွေကို ပိတ်ပစ်ခြင်း၊ အသုံးပြုနေတဲ့ Services တွေကို အနိမ့်ဆုံး အခွင့်အရေး (Least Privilege) နဲ့သာ run စေခြင်းနဲ့ ခွင့်ပြုချက်မရှိဘဲ ပြင်ဆင်လို့မရအောင် ကာကွယ်ခြင်း လုပ်ငန်းစဉ် ဖြစ်ပါတယ်။
---

### **ဘာကြောင့် Services Hardening ကို လုပ်ဆောင်ရတာလဲ? (Importance)**

1. **Attack Surface Reduction (တိုက်ခိုက်နိုင်မည့် လမ်းကြောင်းများ လျှော့ချခြင်း):**
Service တစ်ခု အလုပ်လုပ်နေရင် Port တစ်ခု ပွင့်နေတတ်ပါတယ်။ မလိုအပ်တဲ့ Service တွေကို ပိတ်ထားခြင်းဖြင့် ဟက်ကာတွေ ဝင်ပေါက်ရှာလို့မရအောင် တားဆီးနိုင်ပါတယ်။
2. **Privilege Escalation Prevention (အခွင့်အရေး အလွဲသုံးစားလုပ်မှုကို တားဆီးခြင်း):**
Services တော်တော်များများဟာ `SYSTEM` (အမြင့်ဆုံး အခွင့်အရေး) နဲ့ run လေ့ရှိပါတယ်။ တိုက်ခိုက်သူက အဲဒီ Service တစ်ခုကို ထိန်းချုပ်နိုင်သွားရင် System တစ်ခုလုံးကို Admin Rights နဲ့ ထိန်းချုပ်သွားနိုင်ပါတယ်။
3. **Resource Optimization (စက်စွမ်းဆောင်ရည် ကောင်းမွန်စေခြင်း):**
မလိုအပ်တဲ့ Background အလုပ်တွေကို ပိတ်ထားခြင်းဖြင့် RAM နဲ့ CPU အသုံးပြုမှုကို လျှော့ချနိုင်ပြီး စက်ကို ပိုမိုမြန်ဆန်စေပါတယ်။
---

### **Services Hardening ပြုလုပ်ရာတွင် အသုံးပြုသည့် အခြေခံမူများ (Key Principles)**

* **Disable Unnecessary Services:** အသုံးမပြုတဲ့ Services တွေကို `Disabled` သတ်မှတ်ပါ။ (ဥပမာ - Print မသုံးရင် Print Spooler ကို ပိတ်ခြင်း)။
* **Principle of Least Privilege (PoLP):** Service တစ်ခုကို အမြင့်ဆုံး `SYSTEM` အကောင့်နဲ့ မပေးဘဲ အနိမ့်ဆုံးဖြစ်တဲ့ `Local Service` ဒါမှမဟုတ် `Network Service` အကောင့်တွေနဲ့ပဲ run စေခြင်း။
* **Remove Default Privileges:** Services တွေကနေ မလိုအပ်တဲ့ File access တွေ၊ Registry access တွေကို ဖယ်ရှားခြင်း။
---

### **ဘယ်လိုစတင်မလဲ? (Where to start?)**

Windows မှာ `services.msc` ကို ဖွင့်ပြီး လက်ရှိ run နေတဲ့ Services တွေကို စစ်ဆေးနိုင်ပါတယ်။ ဒါပေမဲ့ လက်တွေ့ Hardening လုပ်တဲ့အခါမှာတော့-

1. **Critical Services:** OS အတွက် မရှိမဖြစ်လိုအပ်တာတွေ (ဥပမာ - RPC, Plug and Play)။
2. **Non-Critical Services:** ပိတ်လိုက်ရင်တောင် OS ကို ထိခိုက်မှုမရှိတဲ့အရာတွေ (ဥပမာ - Bluetooth Support, Remote Registry) ဆိုပြီး ခွဲခြားရပါမယ်။   
---

> **Pro Tip for Repo:** > *"Security is not just about adding features; it’s about removing unnecessary ones."* (လုံခြုံရေးဆိုတာ feature အသစ်တွေ ထပ်ထည့်ဖို့တင်မဟုတ်ဘဲ မလိုအပ်တဲ့ feature တွေကို ဖယ်ရှားပစ်ဖို့လည်း ဖြစ်ပါတယ်။)
---
