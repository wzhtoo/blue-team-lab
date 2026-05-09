# SOC Analyst Lab: Infrastructure & Endpoint Hardening

This laboratory project documents the implementation of network monitoring, security controls, and system hardening following best practices and **CIS Benchmarks**.

## Project Structure Overview

### **01. Monitor Traffic**

Network အတွင်း စီးဆင်းနေတဲ့ Traffic တွေကို စောင့်ကြည့်စစ်ဆေးတဲ့အပိုင်း ဖြစ်ပါတယ်။

* **Tshark:** Network traffic များကို command line မှတစ်ဆင့် capture လုပ်ပြီး analyze လုပ်ခြင်း။
* **Zeek (Bro):** Network security monitoring (NSM) ပြုလုပ်ပြီး traffic logs များ ထုတ်ယူခြင်း။
* **Suricata:** Intrusion Detection System (IDS) အဖြစ် အသုံးပြုပြီး တိုက်ခိုက်မှုများကို alert ထုတ်ပေးခြင်း။

### **02. Firewalls**

Network လုံုံခြုံရေးအတွက် ပထမဆုံး တံတိုင်းအဖြစ် firewall များကို setup လုပ်ခြင်း ဖြစ်ပါတယ်။

* **pfSense:** Enterprise-grade open-source firewall ကို အသုံးပြု၍ network segmentation နှင့် traffic control များ ပြုလုပ်ခြင်း။

### **03. Windows 10 Endpoint**

အသုံးပြုသူများ၏ Windows စက်များကို ခိုင်မာအောင် ပြုလုပ်ခြင်း (Endpoint Hardening) ဖြစ်ပါတယ်။

* **CIS Compliance:** Windows 10 အတွက် CIS Benchmarks အတိုင်း settings များ ညှိနှိုင်းခြင်း။
* **Services Hardening:** မလိုအပ်သော services များကို ပိတ်၍ attack surface ကို လျှော့ချခြင်း။
* **Sysmon Installation:** Advanced logging ပြုလုပ်ရန် Microsoft Sysmon ကို တပ်ဆင်ခြင်း။

### **04. Infrastructure Hardening**

Server များနှင့် အခြေခံအဆောက်အအုံပိုင်းကို CIS စံနှုန်းများဖြင့် ကာကွယ်ခြင်း ဖြစ်ပါတယ်။

* **Linux Hardening:** `auditd` အသုံးပြု၍ Linux server များကို စောင့်ကြည့်ခြင်း။ CIS_Ubuntu_Linux_22.04_LTS_Benchmark_v3.0.0.pdf` ကို အခြေခံထားပါတယ်။

---

## Key Learning Objectives

1. **Visibility:** Network နှင့် Host အဆင့်တွင် ဘာတွေဖြစ်ပျက်နေလဲဆိုတာကို logs များမှတစ်ဆင့် မြင်တွေ့နိုင်ခြင်း။
2. **Hardening:** Default settings များထက် ပိုမိုလုံခြုံအောင် System များကို ပြုပြင်ပြောင်းလဲခြင်း။
3. **Compliance:** ကမ္ဘာ့အဆင့်မီ **CIS Benchmarks** များကို လက်တွေ့ အသုံးချတတ်ခြင်း။
---

## Resources & References

* **Windows 10 CIS Benchmark:** Used for Endpoint hardening sections.
* **Ubuntu 22.04 CIS Benchmark v3.0.0:** Used for Infrastructure hardening (Auditd section).
* **Tool Documentation:** Tshark, Zeek, Suricata, and pfSense manuals.
* **Section 4.1:** Configure System Accounting (auditd) to meet Level 2 (High Security) requirements for both Servers and Workstations.


## Project Status
> **Work in Progress (WIP):**
> This laboratory project is currently **under development**. I am actively testing security controls and documenting the results. More updates on **Active Directory (Keymaster)** and **SIEM integration** will be added soon. Stay tuned!