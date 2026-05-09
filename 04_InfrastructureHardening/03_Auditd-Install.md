# Lab Guide: Accessing Linux Server via SSH and Installing Auditd

ဒီ Lab မှာ Host Machine ကနေတစ်ဆင့် Linux Server ဆီ Remote လှမ်းဝင်ပြီး Security Monitoring အတွက် အရေးကြီးတဲ့ `auditd` ကို တပ်ဆင်သွားမှာ ဖြစ်ပါတယ်။

### **Scenario အချက်အလက်များ:**

* **Server IP:** `192.168.56.101` (via pfSense Network)
* **Username:** `defender`
* **Password:** `12345`
---

### **အဆင့် (၁) - SSH အသုံးပြု၍ Server ထဲသို့ ဝင်ရောက်ခြင်း**

Host Machine (Windows သို့မဟုတ် Linux) ရဲ့ Terminal ကိုဖွင့်ပြီး အောက်ပါ command ကို ရိုက်ထည့်ပါ။

```bash
ssh defender@192.168.56.101
```

* **လုပ်ဆောင်ချက်:** SSH ဆိုတာ Secure Shell ဖြစ်ပြီး Remote Server ကို လုံခြုံစွာ ထိန်းချုပ်ဖို့ သုံးပါတယ်။
* **မှတ်ချက်:** ပထမဆုံးအကြိမ် ဝင်တာဆိုရင် "Are you sure you want to continue connecting?" လို့ မေးပါလိမ့်မယ်။ **`yes`** လို့ ရိုက်ပြီး Enter ခေါက်ပါ။ ပြီးနောက် Password `12345` ကို ရိုက်ထည့်ပါ။

---

### **အဆင့် (၂) - System ကို Update လုပ်ခြင်း**

Package အသစ်တွေ မတင်ခင်မှာ ရှိပြီးသားစာရင်းတွေကို Update ဖြစ်အောင် အရင်လုပ်ရပါမယ်။

```bash
sudo apt update && sudo apt upgrade -y
```

*(ဒီနေရာမှာ `sudo` သုံးထားလို့ password ထပ်မေးရင် `12345` ကိုပဲ ထည့်ပေးပါ)*


### **အဆင့် (၃) - Auditd ကို Install လုပ်ခြင်း**

Linux ရဲ့ အဖြစ်အပျက်မှန်သမျှကို မှတ်တမ်းတင်ပေးမယ့် `auditd` (Audit Daemon) ကို တပ်ဆင်ပါမယ်။

```bash
# auditd တပ်ဆင်ခြင်း
sudo apt install auditd audispd-plugins -y
```

### **အဆင့် (၄) - Auditd အလုပ်လုပ်၊ မလုပ် စစ်ဆေးခြင်း**

တပ်ဆင်ပြီးသွားရင် Service က Run နေပြီလားဆိုတာကို အောက်ပါအတိုင်း စစ်ဆေးနိုင်ပါတယ်။

```bash
sudo systemctl status auditd
```

* **Expected Result:** စာသားတွေထဲမှာ **`active (running)`** ဆိုတာလေးကို စိမ်းစိမ်းလေး တွေ့ရပါလိမ့်မယ်။


### **အဆင့် (၅) - Audit Logs များကို ကြည့်ရှုခြင်း (Testing)**

Auditd က မှတ်တမ်းတင်ထားတာတွေကို ကြည့်ဖို့ `ausearch` သို့မဟုတ် `aureport` command ကို သုံးရပါတယ်။

```bash
# ယနေ့အတွက် အကျဉ်းချုပ် Report ကြည့်ရန်
sudo aureport -summary

# Login ဝင်ထားသည့် မှတ်တမ်းများကို ကြည့်ရန်
sudo aureport -l

```

---

### Summary Table

| Step | Command | Description |
| --- | --- | --- |
| **Connection** | `ssh defender@192.168.56.101` | Accessing the server |
| **Installation** | `sudo apt install auditd` | Installing the Audit tool |
| **Verify Status** | `sudo systemctl status auditd` | Checking if service is live |
| **Monitoring** | `sudo aureport` | Generating audit reports |

---

### Security Note:
> **Important Note:** "လက်တွေ့အပြင်လောက (Production) မှာတော့ Password ကို `12345` လိုမျိုး ရိုးရှင်းတာ မသုံးသင့်ပါဘူး။ **CIS Benchmark** အရ အနည်းဆုံး စာလုံး ၁၄ လုံးနှင့် ရှုပ်ထွေးသော Password များကိုသာ အသုံးပြုရန် အကြံပြုထားပါသည်။"

---

