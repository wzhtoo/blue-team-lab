# Zeek (The Network Security Monitor)

**Zeek** (အရင်က Bro လို့ ခေါ်ပါတယ်) ကတော့ Tshark နဲ့ လုံးဝမတူပါဘူး။ သူက Packet တစ်ခုချင်းစီကို ပြတာထက်၊ Network ထဲမှာ ဘာတွေဖြစ်ပျက်သွားလဲဆိုတဲ့ **"နားလည်ရလွယ်သော မှတ်တမ်း (Metadata Logs)"** တွေကို ထုတ်ပေးတာပါ။

### အဓိက အသုံးပြုပုံ (Use Cases):

- **Traffic Summarization:** Network ချိတ်ဆက်မှုတွေကို Log ဖိုင်များအဖြစ် ပြောင်းလဲပေးခြင်း (ဥပမာ - `conn.log`, `http.log`, `dns.log`)။
- **Anomaly Detection:** ပုံမှန်မဟုတ်တဲ့ Network အပြုအမူတွေကို ခြေရာခံခြင်း။
- **File Extraction:** Network ထဲကနေ ဖြတ်သန်းသွားတဲ့ ဖိုင်တွေကို (ဥပမာ - `.exe`, `.pdf`) အလိုအလျောက် ဆွဲထုတ်သိမ်းဆည်းခြင်း။

### 💡 လက်တွေ့နမူနာ (Example):

Zeek က ထုတ်ပေးတဲ့ `dns.log` ကို ကြည့်ခြင်းအားဖြင့် ဘယ်သူက ဘယ် Domain ကို ရှာခဲ့လဲဆိုတာ ချက်ချင်းသိနိုင်ပါတယ်:

```bash
cat dns.log | zeek-cut query answers
```

> **SOC Insight:** Zeek က "ဘယ်သူက ဘယ်သူနဲ့ စကားပြောသွားလဲ" ဆိုတဲ့ အနှစ်ချုပ်ကို သိချင်တဲ့အခါ သုံးပါတယ်။ ဥပမာ - ကြီးမားတဲ့ `.pcap` ဖိုင်ကြီးကို တစ်ခုချင်း လိုက်မကြည့်နိုင်တဲ့အခါ Zeek နဲ့ Log ထုတ်လိုက်ရင် အလုပ်အများကြီး သက်သာသွားပါတယ်။

---

## Tshark နှင့် Zeek နှိုင်းယှဉ်ချက်

| အချက်အလက်              | Tshark                                               | Zeek                                                       |
| ---------------------- | ---------------------------------------------------- | ---------------------------------------------------------- |
| **အဓိကအမျိုးအစား**     | Packet Analyzer                                      | Network Security Monitor                                   |
| **မြင်ကွင်း (View)**   | Packet level (အသေးစိတ်)                              | Transaction/Log level (အနှစ်ချုပ်)                         |
| **Data Format**        | PCAP                                                 | Structured Logs (TSV/JSON)                                 |
| **အသုံးပြုသည့်အချိန်** | ပြဿနာတစ်ခုကို အမြစ်တွယ်အောင် စစ်ဆေးချိန် (Deep Dive) | Network တစ်ခုလုံးကို စောင့်ကြည့်ချိန် (Monitoring/Hunting) |

### SOC Analyst တစ်ယောက်အနေနဲ့ ဘယ်လိုတွဲသုံးမလဲ?

1. **Zeek** ကို သုံးပြီး တစ်နေ့တာ Traffic ထဲမှာ မသင်္ကာစရာ IP သို့မဟုတ် Domain ပါသလား အရင်ရှာပါ။ (High-level visibility)
2. မသင်္ကာစရာ တွေ့ပြီဆိုရင် အဲဒီအချိန်က Traffic ကို **Tshark** နဲ့ ပြန်ဖွင့်ပြီး Packet တစ်ခုချင်းစီမှာ Hacker က ဘာ Payload တွေ ထည့်ထားလဲဆိုတာ အသေးစိတ် စစ်ဆေးပါ။ (Deep Analysis)

# Installation Zeek

```bash
echo "deb http://download.opensuse.org/repositories/security:/zeek/xUbuntu_22.04/ /" | sudo tee /etc/apt/sources.list.d/zeek.list
```

ဒီ command က Linux (Ubuntu) မှာ software အသစ်တစ်ခု ထည့်သွင်းဖို့အတွက် **"လမ်းကြောင်း (Repository)"** ဖောက်ပေးတဲ့ အရေးကြီးတဲ့ အဆင့်ဖြစ်ပါတယ်။

### ၁။ `echo "deb http://... /"`

- **`echo`** ဆိုတာ သူနောက်မှာ ပါတဲ့ စာသား (URL လိပ်စာ) ကို Terminal မှာ ပြန်ထုတ်ပြခိုင်းတာပါ။
- အဲဒီစာသားကတော့ **Zeek Software** တွေ သိမ်းထားတဲ့ **Server လိပ်စာ (Repository)** ဖြစ်ပါတယ်။ Ubuntu က Zeek ကို ဘယ်နေရာကနေ သွားဒေါင်းရမလဲဆိုတာ သိအောင် ဒီလိပ်စာကို ပေးရခြင်း ဖြစ်ပါတယ်။

### ၂။ `|` (The Pipe)

- ဒါကို **Pipe** လို့ ခေါ်ပါတယ်။ ရှေ့က `echo` နဲ့ ထွက်လာတဲ့ စာသားတွေကို နောက်က command ဆီကို လက်ဆင့်ကမ်း သယ်ဆောင်ပေးလိုက်တာပါ။

### ၃။ `sudo tee /etc/apt/sources.list.d/zeek.list`

ဒီနေရာက အဓိက အလုပ်လုပ်တဲ့ အပိုင်းပါ။

- **`sudo`**: System ဖိုင်တွေကို ပြင်မှာဖြစ်လို့ Root (Admin) အခွင့်အရေးနဲ့ လုပ်ဆောင်တာပါ။
- **`tee`**: ဒီ command က အဓိက အလုပ်နှစ်ခု လုပ်ပါတယ်။

1. ရှေ့ကလာတဲ့ စာသားကို Terminal မှာ ပြပေးတယ်။
2. အဲဒီစာသားကို ဖိုင်တစ်ခုထဲ (ဒီနေရာမှာ `zeek.list` ထဲ) ကို **ရေးထည့် (Write)** ပေးလိုက်တာပါ။

- **`/etc/apt/sources.list.d/zeek.list`**:
- `/etc/apt/sources.list.d/` ဆိုတာ Ubuntu ရဲ့ Software လိပ်စာစာရင်းတွေ သိမ်းတဲ့ ဖိုဒါပါ။
- အဲဒီထဲမှာ `zeek.list` ဆိုတဲ့ ဖိုင်အသစ်လေး တစ်ခု ဆောက်လိုက်တာ ဖြစ်ပါတယ်။

---

### ဘာကြောင့် ဒီလို လုပ်ရတာလဲ? (The Purpose)

`sudo apt install zeek` လို့ ရိုက်လိုက်တဲ့အခါ Ubuntu က သူ့ရဲ့ ပုံမှန်စာရင်းထဲမှာ ရှာကြည့်ပါတယ်။ မတွေ့ပါဘူး။ ဒါကြောင့် အခုလိုမျိုး **External Repository (Third-party)** လိပ်စာကို `zeek.list` ဆိုပြီး သီးသန့် အကြောင်းကြားပေးထားမှသာ Ubuntu က အဲဒီ Server ဆီသွားပြီး Zeek ကို ဒေါင်းလုဒ် ဆွဲပေးနိုင်မှာ ဖြစ်ပါတယ်။

## Next Step 2

```bash
curl -fsSL https://download.opensuse.org/repositories/security:/zeek/xUbuntu_22.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/zeek.gpg > /dev/null
```

ဒီ command ကတော့ အရှေ့မှာလုပ်ခဲ့တဲ့ Repository လိပ်စာကို Ubuntu က **"ယုံကြည်စိတ်ချရကြောင်း (Trusted)"** သတ်မှတ်ပေးဖို့အတွက် **Security Key (GPG Key)** ထည့်သွင်းတဲ့ အဆင့်ဖြစ်ပါတယ်။

SOC Analyst တစ်ယောက်အနေနဲ့ ဒီ command ရဲ့ အလုပ်လုပ်ပုံကို နားလည်ထားတာ ကောင်းပါတယ်၊ ဘာလို့လဲဆိုတော့ ဒါဟာ **Chain of Trust** ကို တည်ဆောက်တဲ့ လုပ်ငန်းစဉ်ဖြစ်လို့ပါ။ အပိုင်းလိုက် ခွဲရှင်းပြပေးပါမယ်။

---

### ၁။ `curl -fsSL https://.../Release.key`

- **`curl`**: အင်တာနက်ပေါ်ကနေ ဖိုင်တွေကို ဒေါင်းလုဒ်ဆွဲတဲ့ tool ပါ။
- **`-fsSL`**: ဒါက flag လေးတွေပါ။
- `-f`: Error တက်ရင် ဘာမှမပြဘဲ တိတ်တိတ်လေးနေဖို့ (Fail silently)။
- `-s`: ဒေါင်းနေတဲ့ ရာခိုင်နှုန်း (Progress bar) ကို မပြဖို့ (Silent)။
- `-S`: Error တက်ရင်တော့ ပြဖို့။
- `-L`: URL က တခြားနေရာကို ပြောင်းသွားရင် (Redirect) လိုက်သွားဖို့။

- ဒီအဆင့်မှာ Zeek ရဲ့ **Public Key (Release.key)** ကို ဒေါင်းလုဒ်ဆွဲလိုက်တာ ဖြစ်ပါတယ်။

### ၂။ `| gpg --dearmor`

- **Pipe (`|`)**: ဒေါင်းလုဒ်ဆွဲလို့ ရလာတဲ့ Key စာသားတွေကို `gpg` command ဆီ ပို့ပေးပါတယ်။
- **`gpg --dearmor`**: အင်တာနက်ကနေ ဒေါင်းလိုက်တဲ့ Key တွေဟာ ပုံမှန်အားဖြင့် လူဖတ်လို့ရတဲ့ စာသား (ASCII Armored) ပုံစံနဲ့ လာတတ်ပါတယ်။ Linux ရဲ့ system က အဲဒီစာသားကို တိုက်ရိုက်မဖတ်ဘဲ **Binary (Computer ဖတ်လို့ရတဲ့ပုံစံ)** ကိုပဲ လိုချင်တာပါ။ ဒါကြောင့် `--dearmor` က Binary အဖြစ် ပြောင်းလဲပေးလိုက်တာ ဖြစ်ပါတယ်။

### ၃။ `| sudo tee /etc/apt/trusted.gpg.d/zeek.gpg > /dev/null`

- **`sudo tee .../zeek.gpg`**: Binary ပြောင်းလိုက်တဲ့ Key ကို Ubuntu ရဲ့ **Trusted GPG keys** သိမ်းတဲ့ နေရာထဲမှာ `zeek.gpg` ဆိုတဲ့ ဖိုင်နာမည်နဲ့ သိမ်းလိုက်တာပါ။
- **`> /dev/null`**: ဒီနေရာမှာ `tee` က output ကို terminal မှာလည်း ပြတတ်ပါတယ်။ အဲဒီ Binary စာသားတွေ (စာထူးစာဆန်းတွေ) terminal မှာ ပေါ်မလာအောင် **အမှိုက်ပုံးထဲ ပစ်ထည့် (Discard)** လိုက်တာ ဖြစ်ပါတယ်။

---

### ဘာကြောင့် ဒီအဆင့်က အရေးကြီးတာလဲ?

ဒီ Key ကို မထည့်ရင် `sudo apt update` လုပ်တဲ့အခါ **"GPG error: The following signatures couldn't be verified"** ဆိုတဲ့ Error တက်ပါလိမ့်မယ်။

Ubuntu က ဒီ Key ကို သုံးပြီး သူဒေါင်းလုဒ်ဆွဲမယ့် Zeek software ဟာ လမ်းမှာ Hacker တွေ ကြားဖြတ်ပြင်ဆင်ထားတာ မဟုတ်ဘဲ၊ **Zeek အဖွဲ့အစည်းက တကယ်ထုတ်တာ ဟုတ်မဟုတ်** ကို ဒစ်ဂျစ်တယ်လက်မှတ်နဲ့ စစ်ဆေးနိုင်သွားမှာ ဖြစ်ပါတယ်။

**အနှစ်ချုပ်:** ဒီ command က **"Zeek repository ကနေ ဒေါင်းသမျှ software အားလုံးဟာ တရားဝင်တယ်၊ ယုံကြည်လို့ရတယ်"** လို့ Ubuntu ကို လက်မှတ်ထိုးခိုင်းလိုက်တာ ဖြစ်ပါတယ်။

## Next Setp_3

```bash
sudo apt update
sudo apt install zeek
```

Repository လိပ်စာနဲ့ Key ထည့်ပြီးပြီဆိုရင်တော့ အခုအဆင့်က တကယ့် Software သွင်းတဲ့အဆင့် ဖြစ်ပါတယ်။ တစ်ခုချင်းစီကို ရှင်းပြပေးပါမယ်။

---

### ၁။ `sudo apt update`

ဒီ command က software အသစ်တစ်ခုခုမသွင်းခင် အမြဲတမ်းလုပ်ရမယ့် အဆင့်ဖြစ်ပါတယ်။

- **အလုပ်လုပ်ပုံ:** စက်ထဲမှာရှိတဲ့ Software စာရင်း (Package Index) တွေကို အင်တာနက်က Server တွေဆီသွားပြီး အသစ်ဖြစ်အောင် (Refresh) လုပ်တာပါ။
- **Zeek အတွက် အရေးပါပုံ:** အရှေ့မှာ ထည့်ခဲ့တဲ့ `zeek.list` လိပ်စာကို Ubuntu က အခုမှ စတင်ဖတ်မှာဖြစ်ပါတယ်။ ဒီ command ကို run လိုက်မှသာ **"ဪ... Zeek သွင်းချင်ရင် ဒီလိပ်စာမှာ သွားယူရမှာပဲ"** ဆိုတာကို Ubuntu က သိသွားမှာ ဖြစ်ပါတယ်။

### ၂။ `sudo apt install zeek`

ဒါကတော့ Zeek ကို အမှန်တကယ် Install လုပ်တဲ့ command ပါ။

- **အလုပ်လုပ်ပုံ:** Ubuntu က Zeek Repository ဆီကနေ လိုအပ်တဲ့ file တွေကို ဒေါင်းလုဒ်ဆွဲမယ်၊ ပြီးရင် ့စက်ထဲမှာ အသင့်သုံးနိုင်အောင် နေရာချ (Install) ပေးမှာဖြစ်ပါတယ်။
- **သတိပြုရန်:** Install လုပ်နေစဉ်အတွင်း Terminal မှာ ခရမ်းရောင် Screen နဲ့ **Postfix Configuration** (Mail Server) အကြောင်း မေးလာနိုင်ပါတယ်။ Zeek က Alert တွေကို အီးမေးလ်နဲ့ ပို့ချင်တဲ့အခါ သုံးဖို့အတွက် မေးတာပါ။ က စမ်းသပ်ရုံပဲဆိုရင် **"No configuration"** ကို ရွေးပြီး Enter ခေါက်လိုက်လို့ ရပါတယ်။

---

### ၃။ Install ပြီးသွားရင် ဘယ်ရောက်သွားမလဲ?

Zeek က ပုံမှန် software တွေလို `/usr/bin` ထဲမှာ မနေဘဲ သူ့ဘာသာသီးသန့်နေရာမှာ နေလေ့ရှိပါတယ်။

- **Default Path:** `/opt/zeek/`
- **Binary (Run ရမယ့်နေရာ):** `/opt/zeek/bin/zeek`

### Install ဖြစ်မဖြစ် စစ်ဆေးနည်း

သွင်းပြီးသွားရင် အောက်ပါ command ကိုရိုက်ပြီး စမ်းကြည့်ပါ:

```bash
/opt/zeek/bin/zeek --version
```

အကယ်၍ `zeek version 5.x.x` သို့မဟုတ် `6.x.x` လို့ ပေါ်လာရင် အောင်မြင်စွာ သွင်းပြီးသွားပါပြီ။

---

### နောက်ထပ်လုပ်ဆောင်ရမယ့်အဆင့် (Next Step)

Zeek သွင်းပြီးပြီဆိုရင် ့မှာရှိတဲ့ `.pcap` ဖိုင်ကို စတင်ပြီး Analyse (ခွဲခြမ်းစိတ်ဖြာ) လုပ်လို့ရပါပြီ။

Analyse လုပ်ဖို့ `.pcap` ဖိုင် ရှိနေပြီလား?** ရှိရင်တော့ အောက်ပါ command နဲ့ စမ်းလို့ရပါပြီ:

```bash
/opt/zeek/bin/zeek -r capture.pcap
```

ဒါဆိုရင် `conn.log`, `dns.log`, `http.log` ဆိုတဲ့ ဖိုင်တွေ ထွက်လာပါလိမ့်မယ်။

# Live Monitoring

```bash
sudo nano /opt/zeek/etc/node.cfg
# Change: interface=enp0s8

sudo nano /opt/zeek/etc/networks.cfg
# Add: 192.168.56.0/24    lab network

sudo /opt/zeek/bin/zeekctl deploy
sudo /opt/zeek/bin/zeekctl status
sudo tail -f /opt/zeek/spool/zeek/conn.log
```

အခုရှင်းပြမယ့် အပိုင်းကတော့ Zeek ကို Static ဖြစ်တဲ့ `.pcap` ဖိုင်တွေဖတ်ရုံတင်မဟုတ်ဘဲ၊ Network Card (Interface) ကနေ ဖြတ်သန်းသွားသမျှ Traffic တွေကို **တိုက်ရိုက်စောင့်ကြည့် (Live Monitoring)** လုပ်ဖို့အတွက် Setup လုပ်တဲ့ လုပ်ငန်းစဉ် ဖြစ်ပါတယ်။

အဆင့်ချင်းစီရဲ့ ရည်ရွယ်ချက်ကို အသေးစိတ် ရှင်းပြပေးပါမယ်။

---

### ၁။ Interface သတ်မှတ်ခြင်း (`node.cfg`)

```bash
sudo nano /opt/zeek/etc/node.cfg
# Change: interface=enp0s8
```

- **ရည်ရွယ်ချက်:** Zeek က ဘယ် Network Card ကနေ လာတဲ့ Traffic ကို ဖမ်းရမလဲဆိုတာ သိဖို့ပါ။
- **ရှင်းပြချက်:** `enp0s8` ဆိုတာ ့စက်ရဲ့ Network Interface နာမည်ပါ။ (ဥပမာ- VirtualBox သုံးရင် Host-only adapter ဖြစ်နိုင်ပါတယ်)။ Zeek ဟာ ဒီ interface ပေါ်က ဖြတ်သန်းသွားတဲ့ Packet တိုင်းကို စောင့်ကြည့်မှာ ဖြစ်ပါတယ်။

### ၂။ ကိုယ်ပိုင် Network ကို သတ်မှတ်ခြင်း (`networks.cfg`)

```bash
sudo nano /opt/zeek/etc/networks.cfg
# Add: 192.168.56.0/24    lab network
```

- **ရည်ရွယ်ချက်:** Zeek က ဘယ် IP တွေဟာ ကိုယ်တွင်း Network (Local) ဖြစ်ပြီး၊ ဘယ် IP တွေဟာ အပြင်ကမ္ဘာ (External) လဲဆိုတာ ခွဲခြားနိုင်ဖို့ပါ။
- **ရှင်းပြချက်:** ဒီလို သတ်မှတ်လိုက်ခြင်းအားဖြင့် Zeek ရဲ့ Log တွေထဲမှာ Local-to-Local ဆက်သွယ်မှုလား၊ ဒါမှမဟုတ် အပြင်က Hacker က ကိုယ့်စက်ကို လာတိုက်ခိုက်တာလားဆိုတာကို ပိုမိုတိကျစွာ ခွဲခြားပြသနိုင်မှာ ဖြစ်ပါတယ်။

### ၃။ Zeek ကို စတင်နှိုးခြင်း (`zeekctl deploy`)

```bash
sudo /opt/zeek/bin/zeekctl deploy
```

- **ရည်ရွယ်ချက်:** အပေါ်က ပြင်ဆင်လိုက်တဲ့ Configuration တွေကို အတည်ပြုပြီး Zeek ကို **စတင်လည်ပတ် (Start)** ခိုင်းတာပါ။
- **ရှင်းပြချက်:** `deploy` command ဟာ `check` (အမှားပါလားစစ်)၊ `install` (config တွေနေရာချ) နဲ့ `start` (ဝန်ဆောင်မှုစတင်) ဆိုတဲ့ အလုပ် ၃ ခုလုံးကို တစ်ခါတည်း လုပ်ပေးလိုက်တာ ဖြစ်ပါတယ်။

### ၄။ အခြေအနေကို စစ်ဆေးခြင်း (`zeekctl status`)

```bash
sudo /opt/zeek/bin/zeekctl status
```

- **ရည်ရွယ်ချက်:** Zeek ပုံမှန် အလုပ်လုပ်နေရဲ့လားဆိုတာ အတည်ပြုဖို့ပါ။
- **ဘာကိုကြည့်ရမလဲ:** Output မှာ `manager`, `proxy`, `worker` စတာတွေဟာ **`running`** ဖြစ်နေရပါမယ်။ အကယ်၍ `crashed` ဖြစ်နေရင် interface နာမည် မှားနေတာမျိုး ဖြစ်နိုင်ပါတယ်။

### ၅။ Live Log ကို စောင့်ကြည့်ခြင်း (`tail -f`)

```bash
sudo tail -f /opt/zeek/spool/zeek/conn.log
```

- **ရည်ရွယ်ချက်:** Network ထဲမှာ အခုလက်ရှိ ဖြစ်ပျက်နေတာတွေကို **Real-time** မြင်ရဖို့ပါ။
- **ရှင်းပြချက်:** \* `tail -f` က ဖိုင်ရဲ့ နောက်ဆုံးစာကြောင်းတွေကို အမြဲ Update ဖြစ်အောင် ပြပေးနေတာပါ။
- **`spool/zeek/conn.log`**: Live Monitoring လုပ်နေတဲ့အချိန်မှာ Log တွေဟာ ဒီလမ်းကြောင်းထဲမှာ ခေတ္တရှိနေတတ်ပါတယ်။
- Network ထဲမှာ Connection အသစ်တစ်ခု ဖြစ်တိုင်း ဒီနေရာမှာ စာကြောင်းအသစ်တွေ တိုးတိုးလာတာကို မြင်ရမှာ ဖြစ်ပါတယ်။

---

### SOC Analyst အတွက် အကြံပြုချက်

Live Monitoring လုပ်နေချိန်မှာ `conn.log` တစ်ခုတည်းတင်မကဘဲ အခြား log တွေကိုလည်း တွဲကြည့်နိုင်ပါတယ်:

- **`dns.log`**: ဘယ်သူတွေ ဘယ် website တွေကို ရှာနေသလဲ။
- **`http.log`**: ဘယ်သူတွေ ဘယ် link တွေကို နှိပ်နေသလဲ။

**သတိပြုရန်:** Live Monitoring လုပ်တဲ့အခါ Interface က Traffic အရမ်းများရင် စက်ရဲ့ CPU/RAM တက်လာနိုင်ပါတယ်။ စမ်းသပ်မှုပြီးသွားလို့ Zeek ကို ပိတ်ချင်ရင်တော့ `sudo /opt/zeek/bin/zeekctl stop` လို့ ရိုက်ရုံပါပဲ။
