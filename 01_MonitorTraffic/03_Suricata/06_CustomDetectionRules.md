### **Insider Threats (အတွင်းလူကြောင့်ဖြစ်သော ခြိမ်းခြောက်မှု)**

ဒါကတော့ ကိုယ့်အဖွဲ့အစည်းထဲက ဝန်ထမ်း၊ ကန်ထရိုက်တာ ဒါမှမဟုတ် Access ရထားတဲ့သူတစ်ယောက်ယောက်ကနေတစ်ဆင့် အချက်အလက်တွေ ခိုးယူတာ ဒါမှမဟုတ် စနစ်ကို ဖျက်ဆီးတာမျိုးပါ။

- **ဥပမာ:** ဝန်ထမ်းတစ်ယောက်က ကုမ္ပဏီရဲ့ အရေးကြီး document တွေကို USB ထဲကူးသွားတာမျိုး။

### **Lateral Movement (ကွန်ရက်အတွင်း ဘေးတိုက်ကူးစက်ခြင်း)**

ဟက်ကာက စက်တစ်လုံးကို ဖောက်ထွင်းပြီးတာနဲ့ အဲဒီစက်ကနေတစ်ဆင့် Network ထဲမှာရှိတဲ့ တခြားစက်တွေကို ဆက်ပြီး ရှာဖွေတိုက်ခိုက်တာကို ခေါ်တာပါ။

- **ဥပမာ:** ရုံးခန်းက ကွန်ပျူတာတစ်လုံးကို malware သွင်းပြီးတာနဲ့ အဲဒီစက်ကနေ Server ခန်းထဲက Server တွေဆီကို လှမ်းပြီး password လိုက်နှိုက်တာမျိုး။

### **Policy Violations (စည်းမျဉ်းချိုးဖောက်မှု)**

ကုမ္ပဏီ ဒါမှမဟုတ် အဖွဲ့အစည်းက သတ်မှတ်ထားတဲ့ လုံခြုံရေး စည်းကမ်းတွေကို မလိုက်နာတာပါ။

- **ဥပမာ:** "ခွင့်ပြုချက်မရှိဘဲ hacking tools တွေ (ဥပမာ- Nmap, Wireshark) မသုံးရ" လို့ တားမြစ်ထားတာကို သုံးစွဲတာမျိုး။

---

## ၂။ Custom Rules များ ရှင်းလင်းချက်

သင်ရေးထားတဲ့ rule တစ်ခုချင်းစီက တကယ်အသုံးဝင်တဲ့ Real-world detection တွေဖြစ်ပါတယ်။

### **Rule 1: Scripted HTTP Access (curl/wget စစ်ဆေးခြင်း)**

```suricata
alert http any any -> $HOME_NET any (msg:"SUSPICIOUS - Non-Browser HTTP Client Detected"; flow:established,to_server; content:"curl"; http_user_agent; nocase; classtype:bad-unknown; sid:1000001; rev:1;)
```

- **အဓိပ္ပာယ်:** ပုံမှန်လူတွေက Browser (Chrome, Firefox) သုံးပြီး Website ကြည့်ကြပေမယ့် ဟက်ကာတွေက `curl` ဒါမှမဟုတ် `wget` လို command တွေသုံးပြီး malware ဒေါင်းလုဒ်ဆွဲလေ့ရှိပါတယ်။
- **ဘယ်လိုစစ်သလဲ:** HTTP ရဲ့ `User-Agent` ထဲမှာ `curl` ဆိုတဲ့ စာသားပါလာရင် ချက်ချင်း Alert ပေးမှာပါ။

### **Rule 2: Internal Host Scanning (စက်အချင်းချင်း လိုက်စစ်တာကို ဖမ်းခြင်း)**

```suricata
alert tcp $HOME_NET any -> $HOME_NET any (msg:"LATERAL MOVEMENT - Internal Host Scanning"; flags:S; detection_filter:track by_src, count 50, seconds 30; classtype:attempted-recon; sid:1000002; rev:2;)
```

- **အဓိပ္ပာယ်:** ဒါက **Lateral Movement** ကို ဖမ်းတာပါ။ Network ထဲက စက်တစ်လုံးကနေ တခြားစက်တွေကို (၃၀) စက္ကန့်အတွင်း အကြိမ် (၅၀) ထက်မနည်း ဆက်သွယ်ဖို့ ကြိုးစားရင် (Port Scan လုပ်ရင်) ဒါဟာ မရိုးသားဘူးလို့ သတ်မှတ်ပါတယ်။
- **ဘယ်လိုစစ်သလဲ:** `flags:S` (TCP SYN packet) တွေကို ရေတွက်ပြီး Threshold ကျော်ရင် Alert ထွက်လာမှာပါ။

### **Rule 3: SMB Port Access (Ransomware ကာကွယ်ခြင်း)**

```suricata
alert tcp any any -> $HOME_NET 445 (msg:"CRITICAL - SMB Port Access Detected"; flow:to_server; flags:S; classtype:attempted-recon; sid:1000003; rev:2;)
```

- **အဓိပ္ပာယ်:** Port **445** ဆိုတာ File sharing လုပ်တဲ့ (SMB) port ပါ။ WannaCry လိုမျိုး Ransomware တွေက ဒီ port ကနေတစ်ဆင့် တစ်စက်နဲ့တစ်စက် ကူးစက်တတ်ပါတယ်။
- **ဘယ်လိုစစ်သလဲ:** ပြင်ပကနေဖြစ်စေ၊ အတွင်းကနေဖြစ်စေ Port 445 ကို လှမ်းပြီး ဆက်သွယ်လာတာနဲ့ "CRITICAL" အနေနဲ့ သတိပေးမှာ ဖြစ်ပါတယ်။

---

### **ရှေ့ဆက်လုပ်ဆောင်ရမည့်အဆင့်**

ဒီ rules တွေကို `custom.rules` ဖိုင်ထဲ ထည့်ပြီးရင် Suricata က ဒါတွေကို ဖတ်နိုင်ဖို့အတွက် အောက်ပါအဆင့်တွေကို လုပ်ပေးပါ-

1. **Suricata.yaml ထဲမှာ လမ်းကြောင်းထည့်ပါ:**

   ```bash
   sudo nano /var/lib/suricata/rules/custom.rules
   ```

## Rules

```bash
# =====================================================
# CUSTOM DETECTION RULES - Cyber Defense Mastery Lab
# =====================================================

# Rule 1: Detect scripted HTTP access (curl, wget, scripts)
# Real-world: Attackers use automated tools, not browsers
alert http any any -> $HOME_NET any (msg:"SUSPICIOUS - Non-Browser HTTP Client Detected"; flow:established,to_server; content:"curl"; http_user_agent; nocase; classtype:bad-unknown; sid:1000001; rev:1;)

# Rule 2: Detect lateral movement (internal host scanning internal host)
# Real-world: Ransomware spreads by scanning internal network
alert tcp $HOME_NET any -> $HOME_NET any (msg:"LATERAL MOVEMENT - Internal Host Scanning"; flags:S; detection_filter:track by_src, count 50, seconds 30; classtype:attempted-recon; sid:1000002; rev:2;)

# Rule 3: Detect SMB port access (WannaCry/NotPetya pattern)
# Real-world: Ransomware targets port 445 for propagation
alert tcp any any -> $HOME_NET 445 (msg:"CRITICAL - SMB Port Access Detected"; flow:to_server; flags:S; classtype:attempted-recon; sid:1000003; rev:2;)
```
---

```bash
sudo nano /etc/suricata/suricata.yaml
```

ကိုပြင်ပြီး

`rule-files:` အောက်မှာ `- custom.rules` ပါနေရပါမယ်။

2. **Configuration ကို စစ်ဆေးပါ:**

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml -v
```

3. **Suricata ကို Restart ချပါ:**

```bash
sudo systemctl restart suricata
```
