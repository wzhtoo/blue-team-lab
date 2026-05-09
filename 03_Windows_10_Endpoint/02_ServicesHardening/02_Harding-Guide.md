# *Hardening Guide: Disabling Print Spooler Service

### **Print Spooler ဆိုတာဘာလဲ?**

Print Spooler ဆိုတာ Windows မှာ စာရွက်စာတမ်းတွေ Print ထုတ်တဲ့အခါ အသုံးပြုတဲ့ Background Service တစ်ခုဖြစ်ပါတယ်။ သူက ပရင်တာဆီ ဒေတာတွေမပို့ခင် ယာယီသိမ်းဆည်းပေးထားတဲ့ အလုပ်ကို လုပ်ပါတယ်။

### **ဘာကြောင့် Print Spooler ကို ပိတ်ရတာလဲ? (Security Risk)**

1. **PrintNightmare Vulnerability:** Print Spooler ဟာ Windows ရဲ့ သမိုင်းတစ်လျှောက်မှာ လုံခြုံရေးအားနည်းချက် (Exploit) တွေ အများဆုံးရှိခဲ့တဲ့ အစိတ်အပိုင်းတစ်ခုပါ။ ဟက်ကာတွေက ဒီ Service ကို အသုံးချပြီး System တစ်ခုလုံးကို Admin Rights နဲ့ ထိန်းချုပ်သွားနိုင်ပါတယ်။
2. **Attack Surface Reduction:** တကယ်လို့ Server ဒါမှမဟုတ် PC က Print ထုတ်ဖို့ မလိုအပ်ဘူးဆိုရင် (ဥပမာ- Domain Controller သို့မဟုတ် Web Server) ဒီ Service ကို ဖွင့်ထားခြင်းက ဟက်ကာအတွက် အပေါက်တစ်ခု ဖွင့်ပေးထားသလို ဖြစ်နေပါတယ်။
---

### **Print Spooler ကို ပိတ်နည်း အဆင့်ဆင့် (Manual Method)**

၁။ ပထမဦးဆုံး `Win + R` ကိုနှိပ်ပြီး **`services.msc`** လို့ ရိုက်ထည့်ကာ Enter ခေါက်ပါ။

၂။ ပေါ်လာတဲ့ Services စာရင်းထဲမှာ **Print Spooler** ကို ရှာပါ။

၃။ **Print Spooler** ပေါ်မှာ Right-click နှိပ်ပြီး **Properties** ကို ရွေးပါ။

၄။ **General** tab အောက်က **Startup type** နေရာမှာ **`Disabled`** ကို ရွေးချယ်ပါ။

၅။ အကယ်၍ Service က Run နေသေးတယ်ဆိုရင် **Service status** နေရာက **`Stop`** ခလုတ်ကို နှိပ်ပါ။

၆။ ပြီးရင် **Apply** နှင့် **OK** ကို နှိပ်ပြီး ပိတ်လိုက်ပါ။

---

### **PowerShell ကို အသုံးပြု၍ ပိတ်နည်း (For Automation)**

```powershell
# Print Spooler service ကို ရပ်တန့်ခြင်း
Stop-Service -Name "Spooler" -Force

# Startup type ကို Disabled ပြောင်းလဲခြင်း
Set-Service -Name "Spooler" -StartupType Disabled
```
---

### **စစ်ဆေးနည်း (Verification)**

Service ပိတ်သွားပြီလားဆိုတာကို အောက်ပါ Command နဲ့ ပြန်စစ်နိုင်ပါတယ်-

```powershell
Get-Service -Name "Spooler" | Select-Object Name, Status, StartType
```
---

###  Summary Table

| Component | Setting | Recommendation |
| --- | --- | --- |
| **Service Name** | Print Spooler (Spooler) | **Disable** |
| **Vulnerability** | CVE-2021-34527 (PrintNightmare) | Critical Risk |
| **Use Case** | Servers / Non-printing PCs | Mandatory Hardening |

---

> **အရေးကြီးမှတ်ချက်:**
> *"Print Spooler ကို ပိတ်လိုက်ရင် PDF print ထုတ်တာနဲ့ Physical Printer သုံးတာတွေ လုပ်လို့ရတော့မှာ မဟုတ်ပါဘူး။ ဒါကြောင့် ပရင်တာ တကယ်မလိုတဲ့ Server တွေမှာပဲ အဓိက လုပ်ဆောင်သင့်ပါတယ်"* 


## **Hardening Guide: Disabling Xbox Services**

Windows မှာ Xbox နဲ့ ပတ်သက်တဲ့ Services အဓိက (၄) ခု ရှိပါတယ်။ ဒါတွေဟာ Gaming မဆော့တဲ့ PC တွေမှာ နောက်ကွယ်ကနေ အမြဲ Run နေပြီး System Resources တွေကို သုံးစွဲနေတတ်ပါတယ်။

### **ပိတ်ရမည့် Xbox Services များ**

1. **Xbox Accessory Management Service:** Xbox controller တွေနဲ့ ဆက်စပ်ပစ္စည်းတွေကို စီမံပေးတာပါ။
2. **Xbox Live Auth Manager:** Xbox Live ကို Login ဝင်ဖို့အတွက် သုံးတာပါ။
3. **Xbox Live Game Save:** Game data တွေကို Cloud ပေါ်မှာ သိမ်းဆည်းဖို့ သုံးတာပါ။
4. **Xbox Live Networking Service:** Xbox Live ရဲ့ Network ချိတ်ဆက်မှုတွေကို ကိုင်တွယ်တာပါ။

---

### **နည်းလမ်း (၁) - `services.msc` ကို အသုံးပြု၍ ပိတ်နည်း**

၁။ `Win + R` ကိုနှိပ်ပြီး **`services.msc`** လို့ ရိုက်ထည့်ပါ။
၂။ အောက်ဆုံးနားထိ ဆွဲဆင်းပြီး **"Xbox"** လို့ အစပြုတဲ့ Service ၄ ခုလုံးကို ရှာပါ။
၃။ တစ်ခုချင်းစီပေါ်မှာ Right-click နှိပ်ပြီး **Properties** ကို သွားပါ။
၄။ **Startup type** မှာ **`Disabled`** ကို ပြောင်းပါ။
၅။ Service က Run နေရင် **`Stop`** ကို နှိပ်ပြီး **OK** ပေးပါ။

---

### **နည်းလမ်း (၂) - PowerShell ကို အသုံးပြု၍ ပိတ်နည်း (အမြန်ဆုံးနည်း)**

```powershell
# Xbox Services စာရင်းကို သတ်မှတ်ခြင်း
$xboxServices = @(
    "XboxGipSvc",    # Xbox Accessory Management Service
    "XblAuthManager", # Xbox Live Auth Manager
    "XblGameSave",    # Xbox Live Game Save
    "XboxNetApiSvc"   # Xbox Live Networking Service
)

# တစ်ခုချင်းစီကို ရပ်တန့်ပြီး Startup Type ကို Disabled ပြောင်းခြင်း
foreach ($service in $xboxServices) {
    if (Get-Service -Name $service -ErrorAction SilentlyContinue) {
        Stop-Service -Name $service -Force -ErrorAction SilentlyContinue
        Set-Service -Name $service -StartupType Disabled
        Write-Host "Disabled: $service" -ForegroundColor Green
    }
}
```

---

### **ဘာကြောင့် ဒါကို Hardening တစ်ခုအနေနဲ့ လုပ်သင့်တာလဲ?**

* **Zero-Trust Policy:** လုံခြုံရေးအရ "အသုံးမပြုတဲ့ အရာမှန်သမျှ ပိတ်ထားရမယ်" ဆိုတဲ့ မူဝါဒကြောင့် ဖြစ်ပါတယ်။
* **Security Vulnerabilities:** Xbox Services တွေဟာလည်း Software တွေဖြစ်တဲ့အတွက် နောင်တစ်ချိန်မှာ အားနည်းချက် (Vulnerability) တွေ ပေါ်လာနိုင်ပါတယ်။ မလိုအပ်ဘဲ ဖွင့်ထားခြင်းက အန္တရာယ်ကို ဖိတ်ခေါ်နေသလို ဖြစ်စေပါတယ်။
* **Background Activity:** Xbox Services တွေဟာ Microsoft server တွေနဲ့ အမြဲတမ်း ဆက်သွယ်ဖို့ ကြိုးစားနေတတ်တဲ့အတွက် Network traffic ကို သက်သာစေပါတယ်။
---

### **စစ်ဆေးနည်း (Verification)**

PowerShell မှာ အောက်ပါအတိုင်း ရိုက်ထည့်ပြီး အကုန်လုံး `Disabled` ဖြစ်မဖြစ် စစ်ဆေးနိုင်ပါတယ်-

```powershell
Get-Service -Name *Xbox*, *Xbl* | Select-Object DisplayName, Status, StartType
```