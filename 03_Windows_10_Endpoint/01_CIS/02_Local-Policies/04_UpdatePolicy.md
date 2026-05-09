# GPupdate

Group Policy တွေကို `gpedit.msc` ကနေ ပြင်ပြီးတာနဲ့ တစ်ခါတည်း သက်ရောက်မှု (Effective) ဖြစ်သွားအောင် PowerShell ကနေ လုပ်ဆောင်ရမယ့် နည်းလမ်း။

ပုံမှန်အားဖြင့် Windows က Policy တွေကို နောက်ကွယ်ကနေ မိနစ် ၉၀ ကနေ ၁၂၀ ကြား တစ်ခါ Auto-update လုပ်ပေးပါတယ်။ ဒါပေမဲ့ ကျနော်တို့လို Security သမားတွေကတော့ ပြင်ပြီးတာနဲ့ ချက်ချင်း အလုပ်လုပ်စေချင်တဲ့အတွက် Manual Update လုပ်ရပါတယ်။

---

### ၁။ `gpupdate` Command ကို အသုံးပြုခြင်း

PowerShell ကို **Run as Administrator** နဲ့ ဖွင့်ပြီး အောက်ပါ Command တွေကို အသုံးပြုနိုင်ပါတယ်-

#### **(က) အခြေခံ Update လုပ်နည်း**

```powershell
gpupdate
```

* **ရှင်းလင်းချက်:** ဒါကတော့ ပြောင်းလဲသွားတဲ့ Policy တွေ (Changed policies) ကိုပဲ ရှာဖွေပြီး Update လုပ်ပေးတာပါ။

#### **(ခ) အတင်းအကျပ် Update လုပ်နည်း (အသုံးအများဆုံး)**

```powershell
gpupdate /force
```

* **ရှင်းလင်းချက်:** ဒါကတော့ Policy အဟောင်းရော၊ အသစ်ရော အကုန်လုံးကို အတင်းအကျပ် (Force) ပြန်လည်သက်ရောက်အောင် လုပ်တာပါ။ `gpedit.msc` မှာ ပြင်ပြီးတိုင်း ဒါကို သုံးတာ အသေချာဆုံးပါပဲ။

#### **(ဂ) Log off သို့မဟုတ် Restart ချခိုင်းခြင်း**

တစ်ချို့သော Policy တွေ (ဥပမာ - User Rights Assignment) က Login ပြန်ဝင်မှ ဒါမှမဟုတ် စက်ပြန်တက်မှ အလုပ်လုပ်တာမျိုး ရှိပါတယ်။ အဲဒီအခါမှာ အောက်ပါအတိုင်း သုံးနိုင်ပါတယ်-

```powershell
gpupdate /force /logoff
# သို့မဟုတ်
gpupdate /force /boot
```

---

### ၂။ Policy အောင်မြင်စွာ Update ဖြစ်၊ မဖြစ် စစ်ဆေးနည်း

Update လုပ်ပြီးရင် တကယ်ရော သက်ရောက်သွားပြီလားဆိုတာကို PowerShell ကနေပဲ ပြန်စစ်လို့ရပါတယ်။

#### **(က) Policy Result ကြည့်ခြင်း (Summary)**

```powershell
gpresult /r
```
* **ရှင်းလင်းချက်:** ဒီ Command က ဘယ် Policy တွေက Applied ဖြစ်နေလဲ၊ ဘယ်အချိန်မှာ နောက်ဆုံး Update ဖြစ်ခဲ့လဲဆိုတာကို အကျဉ်းချုပ် ပြပေးပါတယ်။

#### **(ခ) အသေးစိတ် Report ထုတ်ကြည့်ခြင်း (HTML Format)**

ဒါကတော့ Repo တင်ရင်ဖြစ်ဖြစ်၊ ဆရာ့ကို အစီရင်ခံစာတင်ရင်ဖြစ်ဖြစ် အရမ်းကောင်းပါတယ်။

```powershell
gpresult /h C:\PolicyReport.html
```

* **ရှင်းလင်းချက်:** သင့်ရဲ့ C: drive ထဲမှာ `PolicyReport.html` ဆိုတဲ့ ဖိုင်လေး ထွက်လာပါလိမ့်မယ်။ အဲဒါကို Browser နဲ့ ဖွင့်ကြည့်ရင် ဘယ် Security Setting တွေက **Enabled** ဖြစ်နေလဲဆိုတာကို ဇယားလေးတွေနဲ့ လှလှပပ မြင်ရမှာပါ။
---

### ၃။ PowerShell Module သုံးပြီး Update လုပ်ခြင်း (Advanced)

တစ်ခါတရံ `gpupdate` က အဆင်မပြေရင် PowerShell ရဲ့ ကိုယ်ပိုင် Module ကို သုံးလို့ရပါသေးတယ်-

```powershell
Invoke-GPUpdate -Force
```

---

### Quick Guide

> **How to apply changes immediately?**
> 1. Open **PowerShell** (Run as Administrator).
> 2. Run `gpupdate /force`.
> 3. To verify, run `gpresult /r`.
> 4. If you need a detailed report, use `gpresult /h report.html`.
---
