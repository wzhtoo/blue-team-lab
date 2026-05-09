## Introduction to Fail2Ban

**Fail2Ban** ဆိုတာ Linux server တွေမှာ Log file တွေကို အမြဲစောင့်ကြည့်ပြီး မသင်္ကာဖွယ်ရာ လုပ်ဆောင်ချက်တွေကို အလိုအလျောက် တားဆီးပေးတဲ့ Intrusion Prevention Software တစ်ခုဖြစ်ပါတယ်။

ဥပမာအားဖြင့် - တိုက်ခိုက်သူတစ်ယောက်က server ထဲကို Password မှားယွင်းစွာနဲ့ အကြိမ်ကြိမ် ဝင်ရောက်ဖို့ ကြိုးစားရင် (Brute-force attack)၊ Fail2Ban က အဲဒီလူရဲ့ IP Address ကို Firewall မှာ အလိုအလျောက် Block ပေးလိုက်မှာ ဖြစ်ပါတယ်။

---

### **၁။ Installation နှင့် Service စတင်ခြင်း**

စက်ထဲမှာ Fail2Ban ကို အဆင့်ဆင့် တပ်ဆင်အသက်သွင်းနည်း ဖြစ်ပါတယ်။

* **`sudo apt install fail2ban`**: Software ကို စက်ထဲသို့ တပ်ဆင်ခြင်း။
* **`sudo systemctl start fail2ban`**: Fail2Ban service ကို စတင်အလုပ်လုပ်ခိုင်းခြင်း။
* **`sudo systemctl enable fail2ban`**: စက်ကို Restart ချပြီး ပြန်တက်လာတိုင်း Fail2Ban အလိုအလျောက် ပွင့်လာစေရန် သတ်မှတ်ခြင်း။
* **`sudo systemctl status fail2ban`**: Service တကယ် အလုပ်လုပ်နေ (Running) ဖြစ်မဖြစ် စစ်ဆေးခြင်း။

---

### **၂။ Configuration ချမှတ်ခြင်း (CIS Compliance)**

Fail2Ban မှာ မူလပါတဲ့ `jail.conf` ကို တိုက်ရိုက်မပြင်ဘဲ `jail.local` ဖိုင်ဆောက်ပြီး ပြင်တာဟာ အကောင်းဆုံးဖြစ်ပါတယ်။

**Command:** `sudo nano /etc/fail2ban/jail.local`

**ဖိုင်ထဲတွင် အောက်ပါအတိုင်း CIS Standard နှင့်အညီ ထည့်သွင်းပါ:**

```ini
[DEFAULT]
# IP ကို ဘယ်နှစ်မိနစ် (သို့) ဘယ်နှစ်နာရီ ပိတ်ထားမလဲ (CIS Level 2 အရ အချိန်တိုးထားနိုင်သည်)
bantime  = 1h

# ဘယ်နှစ်မိနစ်အတွင်း မှားယွင်းမှုကို ရေတွက်မလဲ
findtime = 10m

# ဘယ်နှစ်ကြိမ်မှားရင် ပိတ်မလဲ (Max Retry)
maxretry = 5

[sshd]
enabled = true
port    = ssh
logpath = %(sshd_log)s
backend = %(sshd_backend)s

```

* **Default Setting ရှင်းလင်းချက်**: `bantime` က ပိတ်ထားမယ့်ကြာချိန်၊ `findtime` က စောင့်ကြည့်မယ့် အချိန်အပိုင်းအခြား ဖြစ်ပါတယ်။
* **sshd ရှင်းလင်းချက်**: ဒါက SSH login မှားတာတွေကို စောင့်ကြည့်ဖို့ သီးသန့် ဖွင့်ပေးလိုက်တာ (Enable) ဖြစ်ပါတယ်။

---

### **၃။ Status စစ်ဆေးခြင်း**

Config ပြင်ပြီးရင် အသက်ဝင်အောင် လုပ်ဆောင်ရမယ့် အပိုင်းဖြစ်ပါတယ်။

* **`sudo systemctl restart fail2ban`**: ပြင်ဆင်လိုက်တဲ့ settings တွေ အလုပ်လုပ်အောင် Service ကို Restart ချခြင်း။
* **`sudo fail2ban-client status`**: လက်ရှိ စောင့်ကြည့်နေတဲ့ (Active ဖြစ်နေတဲ့) Jails စာရင်းကို ကြည့်ခြင်း။
* **`sudo fail2ban-client status sshd`**: SSH နဲ့ ပတ်သက်ပြီး ဘယ် IP address တွေကို Block (Banned) ထားသလဲဆိုတာကို အသေးစိတ် ကြည့်ခြင်း။

---

### **၄။ Simulation: Brute-force တိုက်ခိုက်မှုကို စမ်းသပ်ခြင်း**

Fail2Ban တကယ်အလုပ်လုပ်၊ မလုပ် သိဖို့အတွက် Fake Log တွေ သုံးပြီး စမ်းသပ်နည်း ဖြစ်ပါတယ်။

* **`sudo bash -c 'for i in {1..5}; do ... >> /var/log/auth.log; done'`**: ဒီ command က SSH login ၅ ကြိမ် ဆက်တိုက် မှားယွင်းသွားတဲ့ ပုံစံမျိုးကို system log (`auth.log`) ထဲမှာ အတင်းသွားရေးခိုင်းလိုက်တာ ဖြစ်ပါတယ်။
* **`sleep`**: Log တွေကို Fail2Ban က ဖတ်ပြီး Firewall မှာ သွားပိတ်ဖို့ စက္ကန့်ပိုင်းလောက် အချိန်စောင့်ခိုင်းခြင်း ဖြစ်ပါတယ်။
* **`sudo fail2ban-client status sshd`**: အပေါ်က command ကြောင့် `192.168.1.100` ဆိုတဲ့ IP သည် **Banned IP list** ထဲ ရောက်သွားတာကို မြင်တွေ့ရပါလိမ့်မယ်။

---

### **၅။ IP ပြန်ဖွင့်နည်း**

* **`sudo fail2ban-client unban --all`**: ပိတ်ထားတဲ့ (Banned လုပ်ထားတဲ့) IP အားလုံးကို ပြန်လည် ခွင့်ပြု (Unban) ပေးလိုက်ခြင်း ဖြစ်ပါတယ်။

---

### Summary Table**

| Task | Command | Goal |
| --- | --- | --- |
| **Protect SSH** | `sshd enabled = true` | SSH ကို Brute-force ကာကွယ်ရန် |
| **Check Banned** | `fail2ban-client status sshd` | ပိတ်ခံထားရသော IP ကို စစ်ရန် |
| **Test Rule** | Append failed logs to `auth.log` | Fail2Ban အလုပ်လုပ်ပုံကို စမ်းရန် |

## Fail2Ban: The Guardian of Your Server Logs

**Fail2Ban** ဆိုတာ "Log-parsing application" တစ်ခုပါ။ သူက System ရဲ့ Log ဖိုင်တွေကို အမြဲဖတ်နေပြီး Login မှားတာမျိုး ဒါမှမဟုတ် ကျူးကျော်ဖို့ ကြိုးစားတဲ့ ပုံစံမျိုး (Patterns) တွေကို လိုက်ရှာပါတယ်။ သတ်မှတ်ထားတဲ့ အကြိမ်ရေထက် ကျော်လွန်ပြီး မှားယွင်းလာတဲ့ IP Address ကိုတွေ့ရင် Firewall (UFW/Iptables) မှာ သွားပြီး အလိုအလျောက် **Block (Ban)** လုပ်ပေးလိုက်တာ ဖြစ်ပါတယ်။

---

### Simulation Command ကို အသေးစိတ် ခွဲခြမ်းစိတ်ဖြာခြင်း**

**"တိုက်ခိုက်သူတစ်ယောက်က SSH Password ကို ၅ ကြိမ် ဆက်တိုက် မှားယွင်းစွာ ရိုက်ထည့်လိုက်တဲ့ ဖြစ်ရပ်"** ကို System က တကယ်ဖြစ်သွားသလိုမျိုး ထင်ယောင်ထင်မှားဖြစ်အောင် Log ဖိုင်ထဲမှာ အတင်းသွားရေးခိုင်းလိုက်တာ ဖြစ်ပါတယ်။

```bash
sudo bash -c 'for i in {1..5}; do echo "$(date "+%b %d %H:%M:%S") $(hostname) sshd[$$]: Failed password for testuser from 192.168.1.100 port 12345 ssh2" >> /var/log/auth.log; done'
```

ဒီ Command ထဲမှာပါတဲ့ အစိတ်အပိုင်းတွေကို တစ်ခုချင်းစီ ရှင်းပြပါ့မယ်-

1. **`sudo bash -c`**:
* `/var/log/auth.log` ဆိုတဲ့ system log ဖိုင်ထဲကို သာမန် user က ရေးခွင့်မရှိပါဘူး။ ဒါကြောင့် root (admin) အခွင့်အရေးနဲ့ bash shell တစ်ခုကို ခေါ်ပြီး ခိုင်းလိုက်တာ ဖြစ်ပါတယ်။


2. **`for i in {1..5}; do ... done`**:
* ဒါက Loop ပတ်တာပါ။ တစ်ကြိမ်တည်း မဟုတ်ဘဲ ၅ ကြိမ် ဆက်တိုက် (၅ ကြိမ်မြောက်မှာ Ban သွားအောင်) အလုပ်လုပ်ခိုင်းတာ ဖြစ်ပါတယ်။


3. **`echo "$(date "+%b %d %H:%M:%S") ..."`**:
* **`date`**: Log ဖိုင်ထဲမှာ အချိန်နဲ့ ရက်စွဲက လက်ရှိအချိန် ဖြစ်နေဖို့ လိုပါတယ်။ ဒါမှ Fail2Ban က အခုလေးတင် ဖြစ်သွားတာလို့ မှတ်ယူမှာပါ။
* **`$(hostname)`**: စက်ရဲ့ နာမည်ကို ထည့်ပေးတာပါ။
* **`sshd[$$]`**: SSH Service ကနေ ဖြစ်ပေါ်လာတဲ့ process တစ်ခုလို့ အယောင်ဆောင်တာပါ။


4. **`"Failed password for testuser from 192.168.1.100..."`**:
* **ဒါဟာ အဓိက အချက်ပါပဲ။** Fail2Ban ရဲ့ `sshd` jail ထဲမှာ ဒီလို စာသားပုံစံမျိုး (Regex) ကို ရှာဖို့ သတ်မှတ်ထားပါတယ်။ "Failed password" ဆိုတဲ့ စာသားနဲ့ IP Address ကို တွေ့တာနဲ့ Fail2Ban က "ဒါဟာ တိုက်ခိုက်မှုပဲ" လို့ စတင်မှတ်သားလိုက်ပါတယ်။


5. **`>> /var/log/auth.log`**:
* ရရှိလာတဲ့ Log စာကြောင်း ၅ ကြောင်းကို `/var/log/auth.log` (Linux ရဲ့ Authentication Log ဖိုင်) ထဲကို သွားပေါင်းထည့် (Append) လိုက်တာ ဖြစ်ပါတယ်။


### Workflow: Command ရိုက်ပြီးနောက် ဘာဖြစ်လာမလဲ?

1. **Writing Logs**: command ရိုက်လိုက်တာနဲ့ Log ဖိုင်ထဲမှာ ၅ ကြိမ် ဆက်တိုက် Login မှားတဲ့ စာကြောင်းတွေ ပေါ်လာမယ်။
2. **Monitoring**: Fail2Ban က Log ဖိုင်ကို စက္ကန့်ပိုင်းအလိုက် ဖတ်နေတဲ့အတွက် "အာ... `192.168.1.100` ကတော့ ၅ ကြိမ်တောင် မှားသွားပြီ" လို့ ချက်ချင်းသိသွားမယ်။
3. **Action**: Fail2Ban က ချက်ချင်းပဲ Firewall (UFW) ဆီကို လှမ်းပြောပြီး `192.168.1.100` ကို Block ခိုင်းလိုက်ပါတယ်။
4. **Result**: `sudo fail2ban-client status sshd` လို့ ရိုက်ကြည့်ရင် `Banned IP list` ထဲမှာ `192.168.1.100` ကို တွေ့ရမှာ ဖြစ်ပါတယ်။


### Brute-force Attack Simulation (Testing)**

Fail2Ban သည် Log ဖိုင်များကို အချိန်နှင့်တပြေးညီ စောင့်ကြည့်နေကြောင်း လက်တွေ့စမ်းသပ်ရန်အတွက် အောက်ပါ Simulation Command ကို အသုံးပြုပါသည်။

```bash
# SSH Login ၅ ကြိမ် ဆက်တိုက် မှားယွင်းသည့် ဖြစ်စဉ်ကို Log ဖိုင်ထဲသို့ အတင်းရေးထည့်ခြင်း
sudo bash -c 'for i in {1..5}; do echo "$(date "+%b %d %H:%M:%S") $(hostname) sshd[$$]: Failed password for testuser from 192.168.1.100 port 12345 ssh2" >> /var/log/auth.log; done'
```
**လုပ်ဆောင်ချက် ရှင်းလင်းချက်:**
ဤ command သည် `/var/log/auth.log` ထဲတွင် တိုက်ခိုက်သူ IP `192.168.1.100` က Password မှားရိုက်နေသကဲ့သို့ ဖန်တီးလိုက်ခြင်းဖြစ်သည်။ Fail2Ban သည် ဤ Log ကို ဖတ်မိသည်နှင့် တစ်ပြိုင်နက် သတ်မှတ်ထားသော `maxretry = 5` ပြည့်သွားသဖြင့် အဆိုပါ IP ကို ချက်ချင်း Block (Ban) လုပ်သွားမည် ဖြစ်သည်။
---
