### အဆင့် (၁) - Host Machine ကနေ Linux Server ကို SSH ချိတ်ဆက်ခြင်း

အရင်ဆုံး Host Machine ရဲ့ Terminal (သို့မဟုတ် PowerShell/CMD) ကိုဖွင့်ပါ။ ပြီးရင် အောက်ပါ command နဲ့ Server ထဲဝင်ပါ-

```bash
ssh username@server_ip_address
```

_ဥပမာ - `ssh admin@192.168.1.50_`

### အဆင့် (၂) - Tshark ကို Remote အနေနဲ့ Install လုပ်ခြင်း

Server ထဲရောက်သွားပြီဆိုရင် အောက်ပါ command တွေကို အစဉ်လိုက် ရိုက်ထည့်ပါ-

1. **Package List ကို Update လုပ်ခြင်း:**

```bash
sudo apt update
```

2. **Tshark ကို Install လုပ်ခြင်း:**

```bash
sudo apt install -y tshark
```

3. **Permissions သတ်မှတ်ခြင်း:**
   Install လုပ်နေတုန်း ခရမ်းရောင် Screen နဲ့ "Should non-superusers be able to capture packets?" လို့မေးရင် **Yes** ကိုရွေးပါ။ ပြီးရင် လက်ရှိ User ကို Wireshark Group ထဲထည့်ပါ-

```bash
sudo usermod -aG wireshark $USER
```

4. **Group Update ဖြစ်စေရန်:**
   ပုံမှန်ဆိုရင် Logout ထွက်ရမှာဖြစ်ပေမယ့် SSH connection မပြတ်ချင်ရင် ဒီ command ကိုသုံးပါ-

```bash
newgrp wireshark
```

### အဆင့် (၃) - SSH ကနေတစ်ဆင့် Live Traffic ကြည့်ခြင်း

အခုဆိုရင် Host Machine ကနေပဲ Server ပေါ်က Traffic တွေကို Monitor လုပ်လို့ရပါပြီ။

**A. Terminal ပေါ်မှာတင် တိုက်ရိုက်ကြည့်ခြင်း:**

```bash
tshark -i eth0 -f "tcp port 80"
```

**B. Host Machine ကနေ သိမ်းဆည်းခြင်း (Very Useful):**
တစ်ခါတလေ Server ပေါ်မှာ နေရာမရှိရင် ဒါမှမဟုတ် Host ပေါ်က Wireshark နဲ့ ပြန်ကြည့်ချင်ရင် SSH ကနေတဆင့် Traffic ကို Host ဆီ တိုက်ရိုက် ဆွဲယူလို့ရပါတယ်။ (ဒါကို Host Machine ရဲ့ Terminal မှာ ရိုက်ရမှာပါ)

```bash
ssh username@server_ip "tshark -i eth0 -w -" > capture_from_server.pcap
```

_ဒီ command က Server ပေါ်မှာ Packet ဖမ်းပြီး Host Machine ထဲကို `.pcap` ဖိုင်အနေနဲ့ တိုက်ရိုက် လှမ်းသိမ်းပေးတာ ဖြစ်ပါတယ်။_

### အဆင့် (၄) - SSH Traffic ကို Filter ထုတ်ခြင်း (အရေးကြီးဆုံးအချက်)

SSH နဲ့ ချိတ်ပြီး Monitor လုပ်နေတာဖြစ်တဲ့အတွက် `tshark` က လုပ်နေတဲ့ SSH traffic တွေကိုပါ ပြန်ဖမ်းမိနေပါလိမ့်မယ် (ဒါကို **Background Noise** လို့ခေါ်ပါတယ်)။ ဒါကြောင့် သင့်ရဲ့ SSH traffic ကို ဖယ်ထုတ်ဖို့ လိုအပ်ပါတယ်။

**SSH Traffic မပါဘဲ Monitor လုပ်နည်း:**

```bash
tshark -i eth0 -f "not port 22"
```

_(Port 22 က SSH သုံးတဲ့ port ဖြစ်လို့ သူ့ကို Not filter ခံထားတာဖြစ်ပါတယ်။)_

### အဆင့် (၅) - SOC Lab အတွက် စမ်းသပ်ကြည့်ရန် နမူနာ (Example)

Server ထဲမှာ Web Server (Apache/Nginx) ရှိတယ်ဆိုပါစို့။ Host ကနေ Monitor လုပ်ကြည့်ပါမယ်။

```bash
# HTTP Request တွေကိုပဲ အသေးစိတ်ကြည့်မယ်
tshark -i eth0 -Y "http.request" -T fields -e http.host -e http.user_agent
```

**SOC Analyst တစ်ယောက်အနေနဲ့ ဒီနည်းလမ်းရဲ့ အားသာချက်က ဘာလဲ?**

Server ဆီကို Physical သွားစရာမလိုဘဲ Remote ကနေ Incident Response လုပ်နိုင်တာ၊ Malware analysis လုပ်ဖို့ Traffic ဖမ်းယူနိုင်တာတွေပဲ ဖြစ်ပါတယ်။

ဒီ command ကတော့ **Tshark** ကို အသုံးပြုပြီး သတ်မှတ်ထားတဲ့ Network Interface တစ်ခုပေါ်က Traffic တွေကို စတင်ပြီး ဖမ်းယူ (Capture) တဲ့ အခြေခံအကျဆုံးနဲ့ အရေးကြီးဆုံး command တစ်ခုဖြစ်ပါတယ်။

```bash
sudo tshark -i enp0s8
```

```markdown
defender@ubuntu-server:~$ sudo tshark -i enp0s8
[sudo] password for defender:
Sorry, try again.
[sudo] password for defender:
Sorry, try again.
[sudo] password for defender:
Running as user "root" and group "root". This could be dangerous.
Capturing on 'enp0s8'
** (tshark:1344) 07:57:14.065730 [Main MESSAGE] -- Capture started.
** (tshark:1344) 07:57:14.066097 [Main MESSAGE] -- File: "/tmp/wireshark_enp0s8HTI2H3.pcapng"
1 0.000000000 192.168.56.101 → 192.168.56.1 SSH 274 Server: Encrypted packet (len=220)
2 0.001183354 192.168.56.1 → 192.168.56.101 TCP 60 58560 → 22 [ACK] Seq=1 Ack=221 Win=253 Len=0
3 0.190373612 0a:00:27:00:00:0d → LLDP_Multicast LLDP 62 LA/WINZAWHTOO MA/0a:00:27:00:00:0d 3601
4 0.746600315 192.168.56.101 → 192.168.56.1 SSH 394 Server: Encrypted packet (len=340)
5 0.789268677 192.168.56.1 → 192.168.56.101 TCP 60 58560 → 22 [ACK] Seq=1 Ack=561 Win=252 Len=0
```

အစိတ်အပိုင်းတစ်ခုချင်းစီကို အသေးစိတ် ခွဲခြားရှင်းပြပေးပါမယ်-

### ၁။ Command အစိတ်အပိုင်းများ

- **`sudo`**: Superuser Do လို့ အဓိပ္ပာယ်ရပါတယ်။ Network Packet တွေကို ဖမ်းယူဖို့အတွက် Network Card (Interface) ကို တိုက်ရိုက် Access လုပ်ခွင့်ရှိရပါမယ်။ ဒါကြောင့် Administrator (Root) privilege လိုအပ်တဲ့အတွက် `sudo` ကို ရှေ့က ခံသုံးရတာဖြစ်ပါတယ်။
- **`tshark`**: Packet analyzer tool ဖြစ်တဲ့ Tshark ကို run မယ်လို့ ခိုင်းတာပါ။
- **`-i`**: ဒါက **Interface** ကို ဆိုလိုတဲ့ flag (သို့မဟုတ်) option ဖြစ်ပါတယ်။ သူ့နောက်မှာ ဘယ် Network Card ကနေ ဖမ်းမလဲဆိုတဲ့ အမည်ကို ထည့်ပေးရပါတယ်။
- **`enp0s8`**: ဒါကတော့ သင်ဖမ်းယူချင်တဲ့ **Network Interface ရဲ့ အမည်** ဖြစ်ပါတယ်။ (ပုံမှန်အားဖြင့် VirtualBox သုံးရင် ဒုတိယမြောက် Network Adapter က ဒီအမည်နဲ့ ပေါ်တတ်ပါတယ်။)

### ၂။ ဒီ Command ကို Run လိုက်ရင် ဘာဖြစ်လာမလဲ?

ဒီ command ကို ရိုက်လိုက်တာနဲ့ Tshark က `enp0s8` interface ပေါ်က ဖြတ်သန်းသွားတဲ့ Packet တိုင်းကို ဖမ်းယူပြီး Terminal screen ပေါ်မှာ **တစ်ကြောင်းချင်းစီ (Summary Line)** ပြပေးနေမှာ ဖြစ်ပါတယ်။

ပြပေးမယ့် အချက်အလက်တွေကတော့-

1. **Time**: Packet မိတဲ့ အချိန်။
2. **Source IP**: ဘယ် IP ကနေ လာသလဲ။
3. **Destination IP**: ဘယ် IP ကို သွားသလဲ။
4. **Protocol**: ဘယ်လို အမျိုးအစားလဲ (TCP, UDP, HTTP, ICMP စသဖြင့်)။
5. **Length**: Packet ရဲ့ အရွယ်အစား။
6. **Info**: အဲဒီ Packet ထဲမှာ ဘာတွေပါသလဲဆိုတဲ့ အကျဉ်းချုပ် (ဥပမာ- [SYN], [ACK], သို့မဟုတ် GET / index.html စသဖြင့်)။

### ၃။ သိထားသင့်တဲ့ အချက်များ

- **Stop လုပ်လိုလျှင်**: `Ctrl + C` ကို နှိပ်ပြီး ဖမ်းတာကို ရပ်နိုင်ပါတယ်။
- **Interface အမည် မသေချာလျှင်**: `tshark -D` ကို ရိုက်ပြီး သင့်စက်မှာရှိတဲ့ interface အမည်တွေကို အရင်စစ်ဆေးနိုင်ပါတယ်။
- **တကယ်လို့ Error တက်ရင်**: "Permission Denied" ပြရင် `sudo` ပါမပါ ပြန်စစ်ပါ။ "Interface not found" ပြရင် `enp0s8` အမည် မှန်မမှန် ပြန်စစ်ပါ။

### ၄။ SOC Analyst တစ်ယောက်အနေနဲ့ ဒီထက်ပိုကောင်းအောင် သုံးနည်း

`sudo tshark -i enp0s8` ချည်းပဲ သုံးရင် traffic တွေက အရမ်းများပြီး ဖတ်ရခက်ပါလိမ့်မယ်။ ဒါကြောင့် အခုလိုမျိုး Filter လေးတွေ တွဲသုံးဖို့ အကြံပြုလိုပါတယ်-

- **IP တစ်ခုတည်းကိုပဲ ကန့်သတ်ကြည့်ချင်ရင်:**

```bash
sudo tshark -i enp0s8 -f "host 192.168.56.101"
```

- **မလိုတဲ့ SSH traffic တွေကို ဖယ်ထားချင်ရင်:**

```bash
sudo tshark -i enp0s8 -f "not port 22"
```

## Network Troubleshooting and Reconnaissance

### ၁။ Command ၏ အဓိပ္ပာယ်

**`sudo tshark -i enp0s8 -f "icmp"`**

```markdown
defender@ubuntu-server:~$ sudo tshark -i enp0s8 -f "icmp"
Running as user "root" and group "root". This could be dangerous.
Capturing on 'enp0s8'
 ** (tshark:1369) 07:58:59.953843 [Main MESSAGE] -- Capture started.
 ** (tshark:1369) 07:58:59.953962 [Main MESSAGE] -- File: "/tmp/wireshark_enp0s8QFWOH3.pcapng"
```

- **`-f "icmp"` (Capture Filter):** ဒါက အဓိကအချက်ပါ။ Network ပေါ်မှာ သွားနေတဲ့ traffic တွေအများကြီးထဲကမှ **ICMP protocol** (ဥပမာ- Ping ရိုက်တာမျိုး) ကိုပဲ သီးသန့်စစ်ထုတ်ပြီး ဖမ်းမယ်လို့ ခိုင်းတာပါ။
- **ICMP ဆိုတာ:** အင်တာနက်ပေါ်မှာ control message တွေ ပို့ဖို့သုံးပါတယ်။ SOC analyst တွေအတွက်တော့ တိုက်ခိုက်သူက network ထဲမှာ ဘယ်စက်တွေရှိသလဲဆိုတာ သိဖို့ **Ping Sweep** လုပ်နေတာကို စောင့်ကြည့်ဖို့ သုံးပါတယ်။

---

### ၂။ Output ထဲက စာသားများကို ရှင်းပြခြင်း

- **`Running as user "root" ... This could be dangerous.`**:
  Tshark ကို root power နဲ့ run တဲ့အခါ လုံခြုံရေးအရ သတိပေးတာပါ။ (တကယ်လို့ Tshark မှာ bug ပါခဲ့ရင် root access ပါသွားနိုင်လို့ပါ)။ Lab ထဲမှာတော့ ဒါကို ဂရုစိုက်စရာမလိုပါဘူး။
- **`Capturing on 'enp0s8'`**:
  သင်သတ်မှတ်ပေးလိုက်တဲ့ Network interface ကတ်ပေါ်မှာ packet စဖမ်းနေပြီလို့ ပြောတာပါ။
- **`File: "/tmp/wireshark_enp0s8QFWOH3.pcapng"`**:
  Tshark က ဖမ်းမိသမျှ packet တွေကို memory ထဲမှာတင်မကဘဲ ယာယီအားဖြင့် `/tmp/` ထဲက `.pcapng` ဖိုင်ထဲမှာ သိမ်းပေးထားတယ်လို့ ပြောတာဖြစ်ပါတယ်။

---

### ၃။ ဒါကို ဘယ်လို စမ်းသပ်မလဲ? (Test Case)

ဒီ command ကို run ထားတုန်းမှာပဲ သင်ယူထားတာကို လက်တွေ့မြင်ရအောင် နောက်ထပ် Terminal တစ်ခုဖွင့် (သို့မဟုတ် အခြားစက်တစ်ခုကနေ) အခုလို ရိုက်ကြည့်ပါ-

```bash
ping 8.8.8.8

```

အဲဒီအခါ Tshark terminal မှာ အခုလိုမျိုး စာကြောင်းတွေ တက်လာပါလိမ့်မယ်-
`1  0.000000  192.168.56.101 -> 8.8.8.8  ICMP 98 Echo (ping) request ...`
`2  0.045000  8.8.8.8 -> 192.168.56.101  ICMP 98 Echo (ping) reply ...`

---

### ၄။ SOC Analyst တစ်ယောက်အနေနဲ့ သိထားသင့်သည်များ

ICMP traffic တွေကို စောင့်ကြည့်ခြင်းအားဖြင့် အောက်ပါတို့ကို သိနိုင်ပါတယ်-

- **Network Scanning:** တစ်စက္ကန့်အတွင်းမှာ မတူညီတဲ့ IP တွေဆီကနေ ICMP Request တွေ အများကြီး လာနေရင် တစ်ယောက်ယောက်က Network ကို scan ဖတ်နေတာ ဖြစ်နိုင်ပါတယ်။
- **C2 Communication (ICMP Tunneling):** တစ်ခါတလေ hacker တွေက firewall တွေကို ကျော်ဖြတ်ဖို့ ICMP packet တွေထဲမှာ data တွေကို ဝှက်ပြီး ပို့လေ့ရှိပါတယ်။

---

### ၅။ နောက်ထပ် စမ်းကြည့်သင့်သည့် Filter များ

ICMP အပြင် အခြား protocol တွေကိုလည်း အခုလို ပြောင်းလဲ စမ်းသပ်နိုင်ပါတယ်။

- **TCP SYN Packet တွေပဲ ကြည့်ချင်ရင် (Port Scanning စစ်ရန်):**
  `sudo tshark -i enp0s8 -f "tcp[tcpflags] & tcp-syn != 0"`
- **UDP Traffic တွေကိုပဲ ကြည့်ချင်ရင်:**
  `sudo tshark -i enp0s8 -f "udp"`

ICMP packet တွေကို ဖမ်းကြည့်တဲ့အခါ terminal မှာ output စာကြောင်းလေးတွေ တက်လာတာကို မြင်ရပြီလားာ? တကယ်လို့ ဘာစာမှ မတက်လာဘူးဆိုရင် ping ရိုက်ပြီး စမ်းသပ်ကြည့်ပါ။

## ၁။ Command ၏ အစိတ်အပိုင်းများ

**`ping -c 5 192.168.56.101`**

```markdown
ubuntu စက်ကိုဖွင့်ပြီး ဒီလိုရိုက်ပေးပါ။
uyanpi@uyanpi:~$ ping -c 5 192.168.56.101
PING 192.168.56.101 (192.168.56.101) 56(84) bytes of data.
64 bytes from 192.168.56.101: icmp_seq=1 ttl=64 time=1.92 ms
64 bytes from 192.168.56.101: icmp_seq=2 ttl=64 time=1.80 ms
64 bytes from 192.168.56.101: icmp_seq=3 ttl=64 time=0.974 ms
64 bytes from 192.168.56.101: icmp_seq=4 ttl=64 time=0.902 ms
64 bytes from 192.168.56.101: icmp_seq=5 ttl=64 time=0.751 ms

--- 192.168.56.101 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4012ms
rtt min/avg/max/mdev = 0.751/1.267/1.916/0.487 ms
uyanpi@uyanpi:~$
```

***ဒါကတော့ သင်စောစောက `tshark` ဖွင့်ထားတဲ့ Server ဆီကို တခြားစက် (Client) တစ်ခုကနေ Network ချိတ်ဆက်မှု ရှိမရှိ စမ်းသပ်လိုက်တာ ဖြစ်ပါတယ်။ SOC Analyst တစ်ယောက်အနေနဲ့ ဒီ `ping` ရဲ့ ရလဒ်ကို အောက်ပါအတိုင်း အသေးစိတ် နားလည်ထားဖို့ လိုပါတယ်။***

- **`ping`**: ICMP Echo Request packet တွေကို ပို့ပြီး တစ်ဖက်စက်က အကြောင်းပြန် (Reply) မပြန် စမ်းသပ်တာပါ။
- **`-c 5`**: Count လို့ အဓိပ္ပာယ်ရပါတယ်။ Packet **၅ ခု**ပဲ ပို့မယ်လို့ ကန့်သတ်လိုက်တာပါ။ (ဒါမပါရင် Linux မှာ အမြဲတမ်း ပို့နေပါလိမ့်မယ်)။
- **`192.168.56.101`**: သင်လှမ်းစစ်ချင်တဲ့ Target IP Address (သင့်ရဲ့ Ubuntu Server IP) ဖြစ်ပါတယ်။

---

### ၂။ Output ထဲက အချက်အလက်များကို ခွဲခြမ်းစိတ်ဖြာခြင်း

- **`64 bytes from ...`**: တစ်ဖက်စက်က အကြောင်းပြန်လိုက်တဲ့ Packet ရဲ့ အရွယ်အစားပါ။
- **`icmp_seq=1`**: ဒါက ပထမဆုံး packet လို့ ပြောတာပါ။ ၅ ခုပို့လို့ ၁ ကနေ ၅ ထိ တက်လာတာဖြစ်ပါတယ်။
- **`ttl=64` (Time To Live)**: Packet တစ်ခုက router ဘယ်နှစ်ခုကို ဖြတ်နိုင်သလဲဆိုတဲ့ သက်တမ်းပါ။ Linux စက်တွေက ပုံမှန်အားဖြင့် `64` ကနေ စလေ့ရှိပါတယ်။ (တကယ်လို့ `ttl=128` ဆိုရင် တစ်ဖက်စက်က Windows ဖြစ်နိုင်ခြေ များပါတယ်။)
- **`time=1.92 ms`**: Packet က သင့်စက်ကနေ ထွက်သွားပြီး တစ်ဖက်က ပြန်ရောက်လာဖို့ ကြာချိန် (Round Trip Time) ဖြစ်ပါတယ်။ နည်းလေ ပိုမြန်လေ ဖြစ်ပါတယ်။

---

### ၃။ Statistics (အနှစ်ချုပ်) ကို ဖတ်ခြင်း

```text
5 packets transmitted, 5 received, 0% packet loss
```

- **Transmitted**: ၅ ခု ပို့ခဲ့တယ်။
- **Received**: ၅ ခုလုံး ပြန်ရတယ်။
- **0% packet loss**: လမ်းမှာ ပျောက်သွားတဲ့ packet မရှိဘူး။ (Network connection ကောင်းမွန်တယ်လို့ ဆိုလိုတာပါ)။

---

### ၄။ Tshark နဲ့ ဘယ်လို ဆက်စပ်သလဲ?

သင် ဒီ `ping` ရိုက်နေတဲ့ အချိန်မှာ တစ်ဖက်က `sudo tshark -i enp0s8 -f "icmp"` ဖွင့်ထားတဲ့ Server ရဲ့ Terminal မှာ Packet လိုင်းလေးတွေ တက်လာတာကို တွေ့ရပါလိမ့်မယ်။

- **Request**: `uyanpi` စက်ကနေ `192.168.56.101` ဆီသွားတာ။
- **Reply**: `192.168.56.101` ကနေ `uyanpi` ဆီ ပြန်လာတာ။

---

## အခုလိုမျိုး end point နှစ်ခု တွေ့တာကို ရှင်းပြပေးပါမယ်။

```markdown
defender@ubuntu-server:~$ sudo tshark -i enp0s8 -f "icmp"
Running as user "root" and group "root". This could be dangerous.
Capturing on 'enp0s8'
 ** (tshark:1427) 08:20:49.145508 [Main MESSAGE] -- Capture started.
 ** (tshark:1427) 08:20:49.145627 [Main MESSAGE] -- File: "/tmp/wireshark_enp0s8ZRA6H3.pcapng"
    1 0.000000000 192.168.56.104 → 192.168.56.101 ICMP 98 Echo (ping) request  id=0x2dc0, seq=1/256, ttl=64
    2 0.000027969 192.168.56.101 → 192.168.56.104 ICMP 98 Echo (ping) reply    id=0x2dc0, seq=1/256, ttl=64 (request in 1)
    3 1.001461882 192.168.56.104 → 192.168.56.101 ICMP 98 Echo (ping) request  id=0x2dc0, seq=2/512, ttl=64
    4 1.001488856 192.168.56.101 → 192.168.56.104 ICMP 98 Echo (ping) reply    id=0x2dc0, seq=2/512, ttl=64 (request in 3)
    5 2.006855655 192.168.56.104 → 192.168.56.101 ICMP 98 Echo (ping) request  id=0x2dc0, seq=3/768, ttl=64
    6 2.006901178 192.168.56.101 → 192.168.56.104 ICMP 98 Echo (ping) reply    id=0x2dc0, seq=3/768, ttl=64 (request in 5)
    7 3.007826692 192.168.56.104 → 192.168.56.101 ICMP 98 Echo (ping) request  id=0x2dc0, seq=4/1024, ttl=64
    8 3.007847218 192.168.56.101 → 192.168.56.104 ICMP 98 Echo (ping) reply    id=0x2dc0, seq=4/1024, ttl=64 (request in 7)
    9 4.012742573 192.168.56.104 → 192.168.56.101 ICMP 98 Echo (ping) request  id=0x2dc0, seq=5/1280, ttl=64
   10 4.012780612 192.168.56.101 → 192.168.56.104 ICMP 98 Echo (ping) reply    id=0x2dc0, seq=5/1280, ttl=64 (request in 9)
```

အခု မြင်နေရတဲ့ Output က SOC Analyst တစ်ယောက်အတွက် အခြေခံအကျဆုံးနဲ့ အရေးကြီးဆုံးဖြစ်တဲ့ **Network Traffic Flow (Traffic လားရာ)** ကို ပြသနေတာဖြစ်ပါတယ်။

## End point နှစ်ခု (IP နှစ်ခု) ကြားမှာ ဘယ်လိုမျိုး အပြန်အလှန် ဆက်သွယ်ခြင်း။

### ၁။ End Point (IP Address) များ၏ အခန်းကဏ္ဍ

- **192.168.56.104 (The Sender/Attacker):** ဒါကတော့ `ping` စရိုက်လိုက်တဲ့ စက် (Client) ဖြစ်ပါတယ်။ Network analysis မှာတော့ ဒါကို **Source** လို့ ခေါ်ပါတယ်။
- **192.168.56.101 (The Target/Server):** ဒါကတော့ သင့်ရဲ့ Ubuntu Server (Tshark run ထားတဲ့စက်) ဖြစ်ပါတယ်။ ဒါကို **Destination** လို့ ခေါ်ပါတယ်။

---

### ၂။ Packet တစ်ကြောင်းချင်းစီ၏ အဓိပ္ပာယ် (Flow Analysis)

Packet တွေကို အစုံလိုက် (Pair) ကြည့်ရင် ပိုနားလည်လွယ်ပါတယ်-

- **Line 1 (Request):** `.104` ကနေ `.101` ဆီကို "မင်းရှိလား" လို့ လှမ်းမေးတာ (**Echo request** )။
- **Line 2 (Reply):** `.101` ကနေ `.104` ဆီကို "ငါရှိတယ်" လို့ ပြန်ဖြေတာ (**Echo reply** )။

> **SOC Insight:** Line 2 ရဲ့ အဆုံးမှာ `(request in 1)` လို့ ပါတာ သတိထားမိမှာပါ။ ဒါဟာ Tshark ကနေ ဒီ Reply ဟာ နံပါတ် (၁) မှာ လာထားတဲ့ မေးခွန်းအတွက် အဖြေဖြစ်တယ်လို့ ချိတ်ဆက်ပြပေးတာ (Protocol Correlation) ဖြစ်ပါတယ်။

---

### ၃။ Column တစ်ခုချင်းစီ၏ အသေးစိတ်ရှင်းလင်းချက်

| Column          | အမည်            | ရှင်းလင်းချက်                                                                                         |
| --------------- | --------------- | ----------------------------------------------------------------------------------------------------- |
| **1, 2, 3...**  | Frame Number    | Packet တွေ ရောက်လာတဲ့ အစဉ်လိုက် နံပါတ်။                                                               |
| **0.000027969** | Time Offset     | ပထမဆုံး packet နဲ့ နောက် packet တွေကြား ကြာချိန် (စက္ကန့်)။                                           |
| **ICMP**        | Protocol        | Network စစ်ဆေးတဲ့ protocol သုံးထားကြောင်း ပြတာ။                                                       |
| **id=0x2dc0**   | Identifier      | ဒီ Ping session တစ်ခုလုံးကို ခွဲခြားဖို့သုံးတဲ့ ID ဖြစ်ပါတယ်။ (Request ရော Reply မှာပါ တူနေရမယ်)။     |
| **seq=1/256**   | Sequence Number | Packet ဘယ်နှစ်ခုမြောက်လဲဆိုတာပါ။ သင် `ping -c 5` လို့ ရိုက်ခဲ့လို့ ၁ ကနေ ၅ အထိ အစဉ်လိုက် တက်သွားတာပါ။ |
| **ttl=64**      | Time To Live    | Packet ရဲ့ သက်တမ်းပါ။ တစ်ဖက်စက်က Linux ဖြစ်ကြောင်း ခန့်မှန်းနိုင်ပါတယ်။                               |

---

### ၄။ SOC Analyst တစ်ယောက်အနေနဲ့ ဘာကို သတိထားကြည့်ရမလဲ?

ပုံမှန် အပြန်အလှန်ဆက်သွယ်မှု (Normal Traffic) ဖြစ်ပါတယ်။ ဒါပေမဲ့ အောက်ပါအတိုင်းတွေ့ရင် **သံသယဖြစ်ဖွယ် (Suspicious)** လို့ သတ်မှတ်နိုင်ပါတယ်-

1. **Request တွေပဲ တက်လာပြီး Reply မပြန်ရင်:** Target စက်က ပိတ်ထားတာ ဒါမှမဟုတ် Firewall က ICMP ကို block ထားတာ ဖြစ်နိုင်ပါတယ်။
2. **IP တစ်ခုတည်းကနေ Request တွေ ရောင်စုံ IP တွေဆီ စက္ကန့်ပိုင်းအတွင်း ပို့နေရင်:** ဒါဟာ **Network Scanning (Reconnaissance)** လုပ်နေတာ ဖြစ်ပါတယ်။
3. **ICMP Packet ရဲ့ Size က 98 ထက် အများကြီး ကြီးနေရင် (ဥပမာ 1400 bytes):** ဒါဟာ ICMP ထဲမှာ Data တွေ ဝှက်ပြီး ပို့နေတဲ့ **ICMP Tunneling** ဖြစ်နိုင်ပါတယ်။

---
