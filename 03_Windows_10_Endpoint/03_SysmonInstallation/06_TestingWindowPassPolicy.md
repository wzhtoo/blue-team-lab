## **Lab Guide: Testing Windows Password Policy**

ဒီ Lab မှာ ကျနော်တို့က **Password Complexity** (ခက်ခဲမှု) နဲ့ **Password Length** (အရှည်) မူဝါဒတွေကို စမ်းသပ်သွားမှာ ဖြစ်ပါတယ်။

### **အဆင့် (၁) - Password Policy သတ်မှတ်ခြင်း**

၁။ `Win + R` ကိုနှိပ်ပြီး **`secpol.msc`** လို့ ရိုက်ထည့်ပါ။
၂။ လမ်းကြောင်းအတိုင်း သွားပါ-

> **Account Policies** > **Password Policy**
> ၃။ အောက်ပါ settings တွေကို CIS စံနှုန်းနဲ့အညီ ပြောင်းလဲကြည့်ပါ-

* **Minimum password length:** `10` characters (အနည်းဆုံး ၁၀ လုံး)။
* **Password must meet complexity requirements:** `Enabled` (ဂဏန်း၊ စာလုံးအကြီးအသေးနှင့် သင်္ကေတများ ပါရမည်)။

---

### **အဆင့် (၂) - Policy ကို Update လုပ်ခြင်း**

Setting တွေ ပြင်ပြီးတာနဲ့ ချက်ချင်းသက်ရောက်စေဖို့ PowerShell (Admin) မှာ အောက်ပါ Command ကို ရိုက်ပါ။

```powershell
gpupdate /force
```
---

### **အဆင့် (၃) - စမ်းသပ်ရန် User အသစ်တစ်ခု ဆောက်ခြင်း**

အခု ကျနော်တို့ သတ်မှတ်လိုက်တဲ့ Policy က အလုပ်လုပ်၊ မလုပ် သိဖို့အတွက် User အသစ်တစ်ခု ဆောက်ကြည့်ပါမယ်။

၁။ PowerShell (Admin) ကိုဖွင့်ပါ။
၂။ **Policy နှင့် မကိုက်ညီသော** Password တစ်ခုပေးပြီး User ဆောက်ကြည့်ပါ (ဥပမာ- password အတိုလေး သုံးကြည့်ပါ)။

```powershell
# အကောင့်အသစ်ဆောက်ရန် (Password ကို '123' ဟု ပေးကြည့်ပါ)
$pass = ConvertTo-SecureString "123" -AsPlainText -Force
New-LocalUser -Name "TestUser01" -Password $pass
```

* **မျှော်လင့်ထားသော ရလဒ် (Expected Result):** Error တက်လာပါလိမ့်မယ်။ "The password does not meet the password policy requirements" ဆိုတဲ့ စာသား ပေါ်လာရပါမယ်။ ဒါဆိုရင် အစ်ကို့ Policy အလုပ်လုပ်နေပါပြီ။

---

### **အဆင့် (၄) - Policy နှင့် ကိုက်ညီသော Password ဖြင့် စမ်းသပ်ခြင်း**

အခုတစ်ခါ Policy နဲ့ ကိုက်ညီတဲ့ (Complexity ရှိတဲ့) Password တစ်ခုနဲ့ ပြန်ဆောက်ကြည့်ပါ။

```powershell
# Password ကို 'P@ssw0rd2026!' ဟု ပေးကြည့်ပါ
$pass = ConvertTo-SecureString "P@ssw0rd2026!" -AsPlainText -Force
New-LocalUser -Name "TestUser01" -Password $pass
```

* **ရလဒ်:** Error မတက်ဘဲ User အောင်မြင်စွာ ဆောက်ပြီးသွားပါလိမ့်မယ်။

---

### **အဆင့် (၅) - Event Viewer တွင် ပြန်လည်စစ်ဆေးခြင်း (Auditing)**

အကယ်၍ `Audit account management` ကို ဖွင့်ထားတယ်ဆိုရင် `eventvwr.msc` ထဲမှာ သွားကြည့်နိုင်ပါတယ်။

၁။ **Security Log** ထဲကို သွားပါ။
၂။ **Event ID 4720** (A user account was created) ကို ရှာကြည့်ပါ။
၃။ အဲဒီမှာ ခုနက အောင်မြင်စွာ ဆောက်လိုက်တဲ့ `TestUser01` အကြောင်း မှတ်တမ်းကို တွေ့ရပါလိမ့်မယ်။

---

### **Testing Checklist Table**

| Test Case | Password Used | Expected Result | Policy Status |
| --- | --- | --- | --- |
| **Short Password** | `123` | **Failure** (Blocked) | Pass ✅ |
| **Simple Password** | `password` | **Failure** (Blocked) | Pass ✅ |
| **Complex Password** | `P@ssw0rd2026!` | **Success** (Created) | Pass ✅ |

---

### **မှတ်ချက်**

> "Password Policy ကို စမ်းသပ်တဲ့အခါ လက်ရှိ User ရဲ့ Password ကို သွားပြောင်းမယ့်အစား **User အသစ်တစ်ခု ဆောက်ကြည့်ခြင်း** က ပိုမိုဘေးကင်းပါတယ်။ Error message ပေါ်လာခြင်းက Policy ဟာ စနစ်တကျ အလုပ်လုပ်နေကြောင်း သက်သေပြတာ ဖြစ်ပါတယ်"

