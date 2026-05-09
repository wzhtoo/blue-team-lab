# Windows Endpoint Monitoring with Sysmon

## **၁။ Sysmon ဆိုတာဘာလဲ?**

**Sysmon (System Monitor)** သည် Microsoft Sysinternals suite တွင်ပါဝင်သော Windows System Service နှင့် Device Driver တစ်ခုဖြစ်သည်။ ၎င်းသည် Windows Event Log ထဲတွင် ပုံမှန်ထက်ပိုမိုအသေးစိတ်သော Activity များ (ဥပမာ - Process Creation, Network Connections နှင့် File Changes) ကို မှတ်တမ်းတင်ပေးသဖြင့် ပြိုင်ဘက်ကင်းသော Visibility ကို ရရှိစေပါသည်။

---

## **၂။ ပြင်ဆင်ရမည့်အရာများ**

### **ဒေါင်းလုဒ်လုပ်ရန်:**

1. **Sysmon Binary** - [Microsoft SysInternals](https://download.sysinternals.com/files/Sysmon.zip)
2. **Sysmon Config** - [SwiftOnSecurity Config](https://github.com/SwiftOnSecurity/sysmon-config) (Security Monitoring အတွက် အကောင်းဆုံး Config ဖြစ်သည်)

### **System Requirements:**

* Windows 10/11 သို့မဟုတ် Windows Server
* Administrator Privileges (Admin အခွင့်အရေးလိုအပ်ပါသည်)

---

## **၃။ အဆင့်ဆင့် Setup လုပ်နည်း**

### **အဆင့် ၁: Sysmon နှင့် Configuration ဖိုင်များ Download လုပ်ခြင်း**

PowerShell (Admin) ကိုဖွင့်၍ အောက်ပါ Command များဖြင့် Folder ဆောက်ပြီး ဒေါင်းလုဒ်ရယူပါ။

```powershell
# Directory အသစ်ဆောက်ခြင်း
mkdir C:\Sysmon
cd C:\Sysmon

# Sysmon (Zip) ကို ဒေါင်းလုဒ်ဆွဲခြင်း
Invoke-WebRequest -Uri "https://download.sysinternals.com/files/Sysmon.zip" -OutFile "Sysmon.zip"

# Extract လုပ်ခြင်း
Expand-Archive -Path "Sysmon.zip" -DestinationPath .

# SwiftOnSecurity ၏ Sysmon Configuration ကို ဒေါင်းလုဒ်ဆွဲခြင်း
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml" -OutFile "sysmonconfig.xml"

```

---

### **အဆင့် ၂: Sysmon ကို Install လုပ်ခြင်း**

Configuration ဖိုင်ကို အသုံးပြု၍ Sysmon ကို စတင်တပ်ဆင်ပါမည်။ (မှတ်ချက် - မိမိစက်သည် 64-bit ဖြစ်ပါက `Sysmon64.exe` ကို အသုံးပြုပါ)

#### **PowerShell (Admin) မှ Install လုပ်နည်း:**

```powershell
cd C:\Sysmon
.\Sysmon64.exe -accepteula -i sysmonconfig.xml
```

* **`-accepteula`**: Microsoft ၏ လိုင်စင်သဘောတူညီချက်ကို အလိုအလျောက် လက်ခံခြင်း။
* **`-i`**: သတ်မှတ်ထားသော configuration ဖိုင်ဖြင့် install လုပ်ခြင်း။

---

### **အဆင့် ၃: Installation အောင်မြင်ကြောင်း စစ်ဆေးခြင်း**

တပ်ဆင်ပြီးနောက် Sysmon Service အလုပ်လုပ်နေပြီလားဆိုသည်ကို အောက်ပါ command ဖြင့် စစ်ဆေးပါ။

```powershell
# Service status စစ်ဆေးရန်
Get-Service Sysmon

# Sysmon မှတ်တမ်းတင်ထားသော Event Logs များ ထွက်မထွက် စစ်ဆေးရန်
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5 | Format-Table TimeCreated, Id, Message -AutoSize
```

**Expected Output:**

```text
Status   Name               DisplayName
------   ----               -----------
Running  Sysmon             System Monitor service

```

---

## **၄။ Sysmon Logs များကို ကြည့်ရှုနည်း**

Sysmon logs များကို GUI အသုံးပြု၍ ကြည့်ရှုလိုပါက:

1. **Event Viewer** (eventvwr.msc) ကိုဖွင့်ပါ။
2. **Applications and Services Logs** -> **Microsoft** -> **Windows** -> **Sysmon** -> **Operational** သို့သွား၍ ကြည့်ရှုနိုင်ပါသည်။

---

### Summary Table

| Component | Task | Command / Path |
| --- | --- | --- |
| **Tool** | Sysmon Installation | `Sysmon64.exe -i config.xml` |
| **Config** | Policy update | `Sysmon64.exe -c config.xml` |
| **Logs** | Log Location | `Microsoft-Windows-Sysmon/Operational` |
| **Service** | Check Status | `Get-Service Sysmon` |

---
