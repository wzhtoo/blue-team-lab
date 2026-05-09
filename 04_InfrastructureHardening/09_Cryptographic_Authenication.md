# Cryptographic SSH Authentication & Hardening

ဤအပိုင်းတွင် SSH Access ကို Password ဖြင့်ဝင်ရောက်ခြင်းအစား ပိုမိုလုံခြုံစိတ်ချရသော **Public Key Infrastructure (PKI)** စနစ်သို့ ပြောင်းလဲခြင်းနှင့် **CIS Security Baseline** များအတိုင်း Configuration ချမှတ်ခြင်းတို့ကို ဆောင်ရွက်မည်။

## **၁။ Configuration Backup ယူခြင်း**

မပြင်ဆင်မီ မူလဖိုင်ကို Backup ယူထားခြင်းသည် အကောင်းဆုံး Practice ဖြစ်သည်။

```bash
# Backup မူရင်းဖိုင်ကို ကူးယူခြင်း
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Backup ဖိုင် ရှိမရှိ စစ်ဆေးခြင်း
ls -lh /etc/ssh/sshd_config*
```

---

## **၂။ Key-Based Authentication Setup**

### **က) Windows Host Machine (PowerShell) တွင် Key ထုတ်ခြင်း**

Windows ဘက်မှနေ၍ လုံခြုံရေးအဆင့်မြင့်မားသော **Ed25519** algorithm ကိုသုံးကာ key ဆောက်ပါမည်။

```powershell
# SSH Key ဆောက်ခြင်း
ssh-keygen -t ed25519 -C "defender@ubuntu-server"

# Public Key ကို ကြည့်ရှုပြီး Copy ကူးရန်
type ~/.ssh/id_ed25519.pub
```

### **ခ) Ubuntu Server ဘက်တွင် Key သွားရောက်ထည့်သွင်းခြင်း**

Copy ရလာသော Public Key ကို Server ရှိ `authorized_keys` ထဲသို့ ထည့်ရပါမည်။

```bash
# SSH folder ဆောက်ခြင်းနှင့် Permission ပေးခြင်း
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# Public Key ကို Paste လုပ်ရန် ဖိုင်ဖွင့်ခြင်း
nano ~/.ssh/authorized_keys

# (Paste the key here, then save and exit)

# ဖိုင်၏ Permission ကို လုံခြုံအောင် ကန့်သတ်ခြင်း
chmod 600 ~/.ssh/authorized_keys

# အောင်မြင်စွာ ထည့်သွင်းပြီးကြောင်း စစ်ဆေးခြင်း
cat ~/.ssh/authorized_keys
```

> ယခုအခါ Server သို့ ပြန်လည်ချိတ်ဆက်ကြည့်လျှင် Password တောင်းတော့မည် မဟုတ်ဘဲ တိုက်ရိုက်ဝင်ရောက်နိုင်မည် ဖြစ်သည်။

---

## **၃။ CIS Security Baseline နှင့်အညီ SSH ကို Hardening လုပ်ခြင်း**

**CIS (Center for Internet Security)** Baseline ဆိုသည်မှာ system တစ်ခုကို တိုက်ခိုက်မှုများမှ ကာကွယ်ရန် ကမ္ဘာ့အဆင့်မီ သတ်မှတ်ထားသော လုံခြုံရေး စံနှုန်းများဖြစ်သည်။

`sudo nano /etc/ssh/sshd_config` ထဲတွင် အောက်ပါအတိုင်း ပြင်ဆင်ပါ-

* **LogLevel INFO**: Log များကို စနစ်တကျ မှတ်တမ်းတင်ရန်။
* **LoginGraceTime 60**: Login ဝင်ချိန်ကို စက္ကန့် ၆၀ သာပေးရန် (Brute force ကာကွယ်ရန်)။
* **PermitRootLogin no**: Root ဖြင့် တိုက်ရိုက်ဝင်ခြင်းကို လုံးဝပိတ်ရန်။
* **MaxAuthTries 4**: Password ခန့်မှန်းမှုကို ၄ ကြိမ်သာ ခွင့်ပြုရန်။
* **MaxSessions 10**: တစ်ပြိုင်တည်း ချိတ်ဆက်နိုင်သည့် အရေအတွက်ကို ကန့်သတ်ရန်။
* **PermitEmptyPasswords no**: Password မပါဘဲ ဝင်ခြင်းကို ခွင့်မပြုရန်။
* **ClientAliveInterval 15** နှင့် **ClientAliveCountMax 3**: အသုံးမပြုဘဲ ငြိမ်နေသော connection များကို ၄၅ စက္ကန့်အကြာတွင် အလိုအလျောက် ဖြတ်တောက်ရန်။

---

## **၄။ Password Authentication ကို အပြီးတိုင်ပိတ်ခြင်း**

Cloud image များတွင် ပါဝင်တတ်သော configuration များကိုပါ ပိတ်ရန် လိုအပ်သည်။

```bash
sudo nano /etc/ssh/sshd_config.d/50-cloud-init.conf

# အောက်ပါအတိုင်း ပြောင်းလဲပါ
PasswordAuthentication no
```

---

## **၅။ Syntax Check နှင့် Service Restart**

Configuration မှားယွင်းပါက Server ထဲဝင်မရဖြစ်နိုင်သဖြင့် Restart မချမီ စစ်ဆေးရမည်။

```bash
# Syntax အမှားရှိမရှိ စစ်ဆေးခြင်း
sudo sshd -t

# အမှားမရှိပါက Service ကို Restart ချခြင်း
sudo systemctl restart sshd
sudo systemctl status sshd
```

---

## **၆။ Final Testing (စမ်းသပ်စစ်ဆေးခြင်း)**

### **စမ်းသပ်မှု (၁) - Password ဖြင့်ဝင်ခြင်းကို ငြင်းပယ်ခြင်း (Force Failure)**

Public Key Authentication ကို အတင်းအကျပ်ပိတ်ပြီး ဝင်ကြည့်ပါမည်။

```bash
ssh -o PubkeyAuthentication=no defender@192.168.56.101
```

* **ရလဒ်:** `Permission denied (publickey)` ဟု ပြပါလိမ့်မည်။
* **ရှင်းလင်းချက်:** Server သည် Password ဖြင့်ဝင်ခြင်းကို လုံးဝလက်မခံတော့ဘဲ (Complete reject) လုံခြုံသော Key ရှိသူကိုသာ လက်ခံတော့မည် ဖြစ်ကြောင်း သက်သေပြခြင်းဖြစ်သည်။

### **စမ်းသပ်မှု (၂) - ပုံမှန်အတိုင်း ချိတ်ဆက်ခြင်း (Success)**

```bash
ssh defender@192.168.56.101
```

* **ရလဒ်:** Password ရိုက်စရာမလိုဘဲ လုံခြုံစိတ်ချစွာဖြင့် Server ထဲသို့ ရောက်ရှိသွားမည်ဖြစ်သည်။
---

