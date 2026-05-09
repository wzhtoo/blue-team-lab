# Suricata

Network Security လောကမှာ အမြဲတွဲဖက်သုံးလေ့ရှိတဲ့ **Suricata** အကြောင်းကို ရှင်းပြပေးပါမယ်။

**Suricata** ဆိုတာ အလွန်စွမ်းအားထက်မြက်တဲ့ open-source **Network Threat Detection Engine** တစ်ခုဖြစ်ပါတယ်။ သူက အဓိကအားဖြင့် အခန်းကဏ္ဍ (၃) ခုမှာ အလုပ်လုပ်ပါတယ်-

1. **IDS (Intrusion Detection System):** ကျူးကျော်သူရှိမရှိ စောင့်ကြည့်အချက်ပေးခြင်း။
2. **IPS (Intrusion Prevention System):** တိုက်ခိုက်မှုကို တွေ့ရှိပါက အလိုအလျောက် တားဆီးပိတ်ပင်ခြင်း။
3. **NSM (Network Security Monitoring):** Network traffic များကို မှတ်တမ်းတင်ခြင်း။

---

### ၁။ Suricata ရဲ့ အဓိက ထူးခြားချက်များ

Zeek နဲ့ ဆင်တူသယောင်ရှိပေမယ့် Suricata မှာ မတူညီတဲ့ အားသာချက်တွေ ရှိပါတယ်-

- **Signature-Based Detection:** Suricata က "Rules" (စည်းမျဉ်းများ) ပေါ်မှာ အခြေခံပါတယ်။ ဥပမာ - "အကယ်၍ Packet ထဲမှာ ဒီလို Malware နဲ့တူတဲ့ စာသားပါလာရင် Alert ပေးပါ" ဆိုတဲ့ ပုံစံမျိုးပါ။
- **Multi-Threading:** သူက စက်ရဲ့ CPU Core အားလုံးကို အပြည့်အဝ အသုံးချနိုင်လို့ Traffic အလွန်များတဲ့ (High-speed) network တွေမှာ Zeek ထက် ပိုပြီး မြန်ဆန်ထိရောက်ပါတယ်။
- **Protocol Identification:** Protocol တွေကို အလိုအလျောက် ခွဲခြားသိမြင်နိုင်စွမ်း ရှိပါတယ်။

---

### ၂။ Suricata ရဲ့ အလုပ်လုပ်ပုံ (Logic)

Suricata က Network ထဲက Packet တွေကို ဖမ်းယူပြီး သူပိုင်ဆိုင်တဲ့ **Rule Set** (စည်းမျဉ်းများစာရင်း) နဲ့ တိုက်စစ်ပါတယ်။

**နမူနာ Rule တစ်ခုကို ကြည့်ရအောင်:**

```bash
alert http $EXTERNAL_NET any -> $HTTP_SERVERS any (msg:"ET MALWARE Potential SQLi Scan"; content:"select"; http_uri; sid:1000001;)
```

- **alert**: ဒီ Rule နဲ့ ကိုက်ညီရင် Alert ထုတ်ပေးပါ။
- **http**: HTTP traffic ကိုပဲ စစ်ပါ။
- **content:"select"**: URL လိပ်စာထဲမှာ `select` ဆိုတဲ့ စာလုံးပါနေရင် (SQL Injection ဖြစ်နိုင်လို့)။
- **msg**: SOC Analyst မြင်ရမယ့် သတိပေးစာသား။

---

### ၃။ Suricata ရဲ့ Output (Log ဖိုင်များ)

Suricata က ခွဲခြမ်းစိတ်ဖြာပြီးရင် `JSON` format နဲ့ Log တွေ ထုတ်ပေးပါတယ်။ အရေးကြီးဆုံးဖိုင်ကတော့ **`eve.json`** ဖြစ်ပါတယ်။

- **`eve.json`**: အားလုံးရဲ့ အနှစ်ချုပ်ပါ။ ဒီဖိုင်ထဲမှာ Alert တွေ၊ DNS query တွေ၊ HTTP request တွေ အကုန်ပါပါတယ်။ (SIEM tool တွေဖြစ်တဲ့ ELK သို့မဟုတ် Splunk ဆီ ပို့ဖို့ အလွန်လွယ်ကူပါတယ်)။
- **`fast.log`**: Alert သီးသန့်ကို စာကြောင်းတိုလေးတွေနဲ့ ပြတဲ့ဖိုင်ပါ။

---

### ၄။ Zeek နဲ့ Suricata ဘာကွာသလဲ?

SOC Analyst တစ်ယောက်အနေနဲ့ ဒီနှစ်ခုရဲ့ ခြားနားချက်ကို သိထားဖို့ အရေးကြီးပါတယ်-

| အချက်အလက်     | Suricata                                             | Zeek                                             |
| ------------- | ---------------------------------------------------- | ------------------------------------------------ |
| **အဓိကအလုပ်** | **Detection** (တိုက်ခိုက်မှုကို ရှာဖွေဖော်ထုတ်ခြင်း) | **Analysis** (ဘာတွေဖြစ်သွားလဲ မှတ်တမ်းတင်ခြင်း)  |
| **နည်းပညာ**   | Signatures (Rules) ကို သုံးသည်                       | Behavioral Analysis (အပြုအမူ) ကို သုံးသည်        |
| **ရလဒ်**      | "Hacker ဝင်နေပြီ" ဆိုတဲ့ Alert ပေးသည်                | "ဘယ်သူနဲ့ ဘယ်သူ စကားပြောနေတယ်" ဆိုတဲ့ Log ပေးသည် |
---

### SOC Analysis မှာ ဘယ်လိုတွဲသုံးမလဲ?

တကယ့်လုပ်ငန်းခွင်မှာတော့ **Suricata + Zeek** ကို တွဲသုံးတာ အကောင်းဆုံးပါ။

1. **Suricata** က "ဒီမှာ Malware နဲ့ တူတဲ့ Traffic တွေ့တယ်!" လို့ အချက်ပေး (Alert) လိုက်မယ်။
2. SOC Analyst က အဲဒီအချိန်မှာ ဘာတွေ တကယ်ဖြစ်သွားသလဲဆိုတာ သိဖို့ **Zeek** ရဲ့ Log တွေကို ပြန်ကြည့်ပြီး Context ကို နားလည်အောင် လုပ်ပါတယ်။
