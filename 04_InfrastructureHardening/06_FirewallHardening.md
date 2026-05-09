# Firewall Hardening*

# Network Hardening with UFW

**UFW (Uncomplicated Firewall)** သည် Linux system များတွင် network traffic ကို ရိုးရှင်းစွာ ထိန်းချုပ်နိုင်ရန် အသုံးပြုသော tool ဖြစ်သည်။ ဤအပိုင်းတွင် system တစ်ခုလုံး၏ network အဝင်/အထွက်ကို လုံခြုံအောင် ပိတ်ဆို့ခြင်းနှင့် လိုအပ်သော port များကိုသာ ဖွင့်လှစ်ခြင်းတို့ကို ဆောင်ရွက်မည်။

### **၁။ အခြေခံ Setup နှင့် Installation**

* **`which ufw`**: System ထဲတွင် UFW ရှိမရှိနှင့် ရှိပါက မည်သည့်လမ်းကြောင်းတွင် ရှိသည်ကို စစ်ဆေးခြင်း။
* **`sudo apt install ufw`**: UFW မရှိပါက Package repository မှ ဒေါင်းလုဒ်ဆွဲ၍ တပ်ဆင်ခြင်း။
---

### **၂။ Default Policy သတ်မှတ်ခြင်း (Standard Hardening)**

Firewall တစ်ခု၏ အခြေခံလုံခြုံရေးအတွက် "အဝင်အားလုံးကိုပိတ်၍ အထွက်အားလုံးကိုဖွင့်ခြင်း" ကို ဦးစွာလုပ်ဆောင်ရမည်။

* **`sudo ufw default deny incoming`**: ပြင်ပမှ မိမိစက်ထဲသို့ လှမ်းဝင်လာမည့် connection အားလုံးကို မူလအားဖြင့် ပိတ်ထားခြင်း (Default Deny)။
* **`sudo ufw default allow outgoing`**: မိမိစက်မှ ပြင်ပ (Internet) သို့ ထွက်မည့် traffic များကို ခွင့်ပြုခြင်း။
* **`sudo ufw default deny routed`**: မိမိစက်ကို ကြားခံ (Router) အဖြစ်သုံး၍ traffic များ ဖြတ်စီးခြင်းကို ပိတ်ထားခြင်း။
---

### **၃။ Specific Rules နှင့် Firewall အသက်သွင်းခြင်း**

Default တွေ ပိတ်ပြီးပါက လိုအပ်သော port ကို သီးသန့်ပြန်ဖွင့်ပေးရမည်။

* **`sudo ufw allow 22/tcp comment 'SSH'`**: SSH အသုံးပြုရန် Port 22 ကို ဖွင့်ပေးခြင်း ဖြစ်သည်။ Comment ထည့်ခြင်းဖြင့် နောင်တစ်ချိန်တွင် ဤ rule ကို အဘယ်ကြောင့် ဖွင့်ခဲ့သည်ကို သိနိုင်စေသည်။
* **`sudo ufw show added`**: Firewall မဖွင့်မီ မိမိထည့်သွင်းထားသော rule များ မှန်ကန်မှု ရှိ၊ မရှိ ပြန်လည်စစ်ဆေးခြင်း။
* **`sudo ufw enable`**: Firewall ကို စတင်အသက်သွင်းခြင်း (ဤနေရာတွင် SSH မဖွင့်ရသေးဘဲ enable လုပ်ပါက connection ပြုတ်သွားနိုင်သဖြင့် သတိပြုပါ)။
* **`sudo ufw status verbose`**: Firewall အလုပ်လုပ်နေမှု အခြေအနေနှင့် ဖွင့်ထားသော rule များကို အသေးစိတ် (Detailed) ထုတ်ကြည့်ခြင်း။
---

### **၄။ Testing & Validation (စမ်းသပ်စစ်ဆေးခြင်း)**

Firewall က တကယ်အလုပ်လုပ်နေပြီလားဆိုသည်ကို `telnet` သုံး၍ စမ်းသပ်ခြင်း ဖြစ်သည်။

* **`timeout 3 telnet localhost 80`**: Port 80 (HTTP) ကို စမ်းသပ်ခြင်း။ ကျွန်ုပ်တို့ ပိတ်ထားသဖြင့် ၃ စက္ကန့်အတွင်း connection မရဘဲ timeout ဖြစ်သွားရမည်။
* **`timeout 3 telnet localhost 22`**: SSH (Port 22) ကို စမ်းသပ်ခြင်း။ ကျွန်ုပ်တို့ rule ဖွင့်ထားသဖြင့် connection ရရှိရမည် (Connected to localhost ပြရမည်)။

---

### Summary Checklist**

| Command | Purpose | Expected Result |
| --- | --- | --- |
| `ufw status` | Check Status | Active/Inactive |
| `ufw allow 22` | Management | SSH Access Granted |
| `telnet <port>` | Verify Rule | Open/Closed status |

---
