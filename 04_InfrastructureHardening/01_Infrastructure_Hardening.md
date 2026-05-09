# Infrastructure Hardening

ကုမ္ပဏီတစ်ခုလုံးရဲ့ စနစ်ကြီးတစ်ခုလုံး (Servers, Networks, Cloud) ကို ခြုံငုံကာကွယ်တာ ဖြစ်ပါတယ်။

## **Hardening Concept: Endpoint to Infrastructure**

လုံခြုံရေးကို အလွှာလိုက် (Defense in Depth) တည်ဆောက်တဲ့နေရာမှာ Hardening ဟာ အခြေခံအကျဆုံးဖြစ်ပါတယ်။

### **၁။ Endpoint Hardening (တစ်ဦးချင်းစက်များကို ခိုင်မာအောင်လုပ်ခြင်း)**

Endpoint ဆိုတာ ဝန်ထမ်းတွေသုံးတဲ့ Laptop, Desktop နဲ့ Mobile ဖုန်းတွေကို ဆိုလိုတာပါ။ တိုက်ခိုက်သူတွေဟာ ဒီ Endpoint တွေကနေတစ်ဆင့် Network ထဲကို စတင်ဝင်ရောက်လေ့ရှိပါတယ်။

* **OS Hardening:** အသုံးမလိုတဲ့ Windows Services တွေကို ပိတ်ခြင်း (ဥပမာ- Print Spooler, Xbox services)။
* **Browser Hardening:** မလုံခြုံတဲ့ Extensions တွေ ပိတ်ခြင်း၊ Pop-ups များကို တားဆီးခြင်း။
* **Local Admin Restriction:** ဝန်ထမ်းများကို Admin Rights မပေးဘဲ Standard User အဖြစ်သာ သုံးစွဲစေခြင်း (ဒါဟာ Malware ပြန့်နှံ့မှုကို ၈၀% ကျော် တားဆီးနိုင်ပါတယ်)။
* **Endpoint Security:** Antivirus/EDR (Endpoint Detection and Response) တပ်ဆင်ခြင်းနှင့် USB ports များကို ပိတ်ထားခြင်း။

---

### **၂။ Infrastructure Hardening (အခြေခံအဆောက်အအုံတစ်ခုလုံးကို ကာကွယ်ခြင်း)**

Endpoint တွေထက် ပိုမိုကျယ်ပြန့်ပြီး ကုမ္ပဏီရဲ့ ဒေတာတွေ စီးဆင်းရာ နေရာတွေကို ကာကွယ်တာ ဖြစ်ပါတယ်။

#### **A. Server Hardening**

* **Role-Based Installation:** Server တစ်ခုကို Web Server လို့ သတ်မှတ်ရင် Web နဲ့ မဆိုင်တဲ့ ဘယ် Service မှ (ဥပမာ- Print, Mail) မရှိစေရပါ။
* **Audit Logging:** Sysmon, Auditd တို့နဲ့ ဘာတွေဖြစ်နေလဲဆိုတာ အမြဲစောင့်ကြည့်ခြင်း။
* **Patch Management:** Vulnerability အသစ်တွေအတွက် Security Patches များကို အမြဲ Update လုပ်ခြင်း။

#### **B. Network Hardening**

* **Firewall (UFW/Windows Firewall):** မလိုအပ်တဲ့ Port တွေအားလုံးကို ပိတ်ထားပြီး လိုအပ်တဲ့ Traffic ကိုပဲ ခွင့်ပြုခြင်း (Default Deny Policy)။
* **VLAN Segmentation:** HR ဌာနရဲ့ Network နဲ့ IT ဌာနရဲ့ Network ကို သီးသန့်စီ ခွဲထုတ်ထားခြင်း (တစ်ဖက်ကို ဟက်ခံရရင် နောက်တစ်ဖက်ကို မကူးစက်အောင် တားဆီးခြင်း)။
* **Disabling Legacy Protocols:** အန္တရာယ်ရှိတဲ့ protocols ဟောင်းတွေ (ဥပမာ- SMBv1, Telnet) ကို ပိတ်ပစ်ခြင်း။

---

### **၃။ Endpoint နှင့် Infrastructure ဆက်စပ်ပုံ (The Connection)**

Hardening ဆိုတာ တစ်ခုတည်း လုပ်ရတာ မဟုတ်ဘဲ အချင်းချင်း ချိတ်ဆက်နေပါတယ်။

* အကယ်၍ **Endpoint** တစ်ခု ဟက်ခံရရင်တောင် **Infrastructure** ဘက်မှာ Network Segmentation ကောင်းကောင်းလုပ်ထားရင် တိုက်ခိုက်သူဟာ Server တွေဆီ ဆက်သွားလို့ မရတော့ပါဘူး။
* ဒါကို **"Attack Surface Reduction"** (တိုက်ခိုက်ခံရနိုင်တဲ့ ဧရိယာကို အတတ်နိုင်ဆုံး လျှော့ချခြင်း) လို့ ခေါ်ပါတယ်။

---

### Summary Comparison Table

| Feature | Endpoint Hardening | Infrastructure Hardening |
| --- | --- | --- |
| **Primary Target** | Laptops, Desktops, Mobiles | Servers, Switches, Firewalls, Cloud |
| **Main Goal** | Prevent user-side infection | Protect data and core services |
| **Tools used** | EDR, GPO, Local Policy | SIEM, VLAN, Firewall, IDS/IPS |
| **Best Practice** | Principle of Least Privilege | Defense in Depth (Layered Security) |

---

### Conclusion:
> "Cybersecurity is not only about having a firewall. It is about **Hardening** every single component of your system. From the laptop on a staff member's desk (Endpoint) to the server room in the data center (Infrastructure), every layer must be secured to create a **Stronghold** for the organization."

