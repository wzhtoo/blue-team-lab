# Basic Firewalls Rules

### Firewall Rules ရေးသားရာတွင် အဓိက ထည့်သွင်းစဉ်းစားရမည့် အချက်များ

*   **လုပ်ဆောင်ချက် (Action)**: ALLOW, DENY, DROP
*   **ပရိုတိုကော (Protocol)**: TCP, UDP, ICMP စသည်
*   **ရင်းမြစ် (Source)**: IP Address သို့မဟုတ် Network (ဥပမာ 192.168.1.10, 10.0.0.0/24)
*   **ဦးတည်ရာ (Destination)**: IP Address သို့မဟုတ် Network
*   **ပြူတင်း (Port)**: Destination Port သို့မဟုတ် Source Port (ဥပမာ 80, 443, 22)

**မှတ်ချက်**: `DENY` ဆိုသည်မှာ ဆက်သွယ်မှုကို ငြင်းဆိုပြီး အကြောင်းကြားချက် ပြန်ပို့ခြင်း၊ `DROP` ဆိုသည်မှာ ဆက်သွယ်မှုကို လုံးဝလျစ်လျူရှုပြီး အကြောင်းမပြန်ခြင်း (စက်သည် စောင့်ဆိုင်းနေရသဖြင့် ပိုလုံခြုံသည်)။

---

### နမူနာ Rules အစုအဝေးများ

#### အုပ်စု ၁: အခြေခံ အင်တာနက်ဝန်ဆောင်မှုများအတွက်
အိမ်သုံး သို့မဟုတ် ရုံးသုံး ကွန်ယက်ငယ်လေးအတွက် အခြေခံ Rules.

1.  **Rule Name**: Allow Outgoing HTTP/HTTPS (Web Browsing)
    *   **Action**: ALLOW
    *   **Protocol**: TCP
    *   **Source**: Internal Network (e.g., 192.168.1.0/24)
    *   **Destination**: Any
    *   **Destination Port**: 80 (HTTP), 443 (HTTPS)
    *   **ရှင်းလင်းချက်**: ကွန်ယက်အတွင်းရှိ စက်များအား ဝက်ဘ်ကြည့်ရန် ခွင့်ပြုသည်။

2.  **Rule Name**: Allow Outgoing DNS
    *   **Action**: ALLOW
    *   **Protocol**: UDP (အဓိက), TCP
    *   **Source**: Internal Network
    *   **Destination**: DNS Server IP (e.g., 8.8.8.8) or Any
    *   **Destination Port**: 53
    *   **ရှင်းလင်းချက်**: ဒိုမိန်းနာမည်များကို IP လိပ်စာသို့ ပြောင်းပေးသော DNS ဝန်ဆောင်မှုကို အသုံးပြုခွင့်ပေးသည်။

3.  **Rule Name**: Block All Incoming from Internet
    *   **Action**: DROP
    *   **Protocol**: Any
    *   **Source**: Any (သို့မဟုတ် WAN Interface)
    *   **Destination**: Internal Network
    *   **Destination Port**: Any
    *   **ရှင်းလင်းချက်**: အင်တာနက်ဘက်မှ ကွန်ယက်အတွင်းသို့ ကြိုတင်ခွင့်ပြုချက်မရှိသော ဆက်သွယ်မှုအားလုံးကို ပိတ်ပင်သည်။ ဤ Rule သည် နောက်ဆုံး (Default Deny) အဖြစ်မကြာခဏ ထားရှိသည်။

#### အုပ်စု ၂: ရုံးလုပ်ငန်းသုံး ဝန်ဆောင်မှုများအတွက်
အသေးစား လုပ်ငန်းတစ်ခု၏ ဆာဗာကို ကာကွယ်ရန် Rules.

4.  **Rule Name**: Allow Incoming Web Server Traffic
    *   **Action**: ALLOW
    *   **Protocol**: TCP
    *   **Source**: Any (သို့မဟုတ် ဖောက်သည်များ၏ IP Range)
    *   **Destination**: Web Server IP (e.g., 203.0.113.10)
    *   **Destination Port**: 80 (HTTP), 443 (HTTPS)
    *   **ရှင်းလင်းချက်**: အင်တာနက်ပေါ်ရှိ မည်သူမဆို ကုမ္ပဏီဝက်ဘ်ဆိုဒ်ကို ဝင်ရောက်ကြည့်ရှုနိုင်သည်။

5.  **Rule Name**: Allow Incoming Secure SSH for Admin
    *   **Action**: ALLOW
    *   **Protocol**: TCP
    *   **Source**: Administrator's Static IP (e.g., 102.129.54.7) **သို့မဟုတ်** VPN Network Only
    *   **Destination**: Server IP
    *   **Destination Port**: 22 (SSH)
    *   **ရှင်းလင်းချက်**: အုပ်ချုပ်သူ (Admin) တစ်ဦးတည်း၏ IP မှသာ ဆာဗာကို Remote ဝင်ရောက်စီမံခွင့် ပေးသည်။ ဤသည်မှာ အလွန်အရေးကြီးသော Rule တစ်ခုဖြစ်သည်။

6.  **Rule Name**: Block Incoming Database Port from Internet
    *   **Action**: DROP
    *   **Protocol**: TCP
    *   **Source**: Any
    *   **Destination**: Database Server IP
    *   **Destination Port**: 3306 (MySQL) / 1433 (MSSQL) / 5432 (PostgreSQL)
    *   **ရှင်းလင်းချက်**: Database ဆာဗာ၏ ပေါ်တ်ကို အင်တာနက်ဘက်မှ တိုက်ရိုက်ဝင်ရောက်ခွင့် ပိတ်ပင်ထားသည်။ Database ကို ဝင်ရောက်ရန် Web Application မှတစ်ဆင့်သာ ခွင့်ပြုသင့်သည်။

#### အုပ်စု ၃: ကာကွယ်ရေးနှင့် စီမံခန့်ခွဲရေး Rules

7.  **Rule Name**: Allow Ping from Internal Network Only
    *   **Action**: ALLOW
    *   **Protocol**: ICMP
    *   **Source**: Internal Network
    *   **Destination**: Any
    *   **ရှင်းလင်းချက်**: ကွန်ယက်အတွင်းမှသာ ဆာဗာများကို Ping ခတ်၍ စစ်ဆေးနိုင်သည်။ အင်တာနက်ဘက်မှ Ping ကို ပိတ်ထားနိုင်သည်။

8.  **Rule Name**: Block Known Malicious IP Range
    *   **Action**: DROP
    *   **Protocol**: Any
    *   **Source**: 192.168.100.0/24 (နမူနာ မကောင်းသော IP အုပ်စု)
    *   **Destination**: Any
    *   **Destination Port**: Any
    *   **ရှင်းလင်းချက်**: ခြိမ်းခြောက်မှု ထောက်လှမ်းရေး မှတ်တမ်းများ (Threat Intel Feeds) မှ သိရှိထားသော အန္တရာယ်ရှိ IP များကို ကြိုတင်ပိတ်ပင်ထားခြင်း။

9.  **Rule Name**: Explicit Deny All (Cleanup Rule)
    *   **Action**: DENY (သို့မဟုတ် DROP)
    *   **Protocol**: Any
    *   **Source**: Any
    *   **Destination**: Any
    *   **Destination Port**: Any
    *   **ရှင်းလင်းချက်**: Firewall Rules စာရင်း၏ အောက်ဆုံးတွင် ဤ Rule ကို ထည့်သွင်းလေ့ရှိသည်။ ၎င်းသည် မည်သည့် Rule နှင့်မှ ကိုက်ညီခြင်းမရှိသော Traffic အားလုံးကို ငြင်းပယ်သည့် "Default Deny" အနေအထားကို သေချာစေသည်။
---

### Rule တစ်ခု၏ အရေးကြီးသော လုပ်ဆောင်ချက် ဥပမာ
**Scenario**: အင်တာနက်ပေါ်တွင် ရှိနေသော ကိုယ်ပိုင် File Server (SFTP) ကို ရုံးမှသာ ဝင်ရောက်နိုင်စေလို။

*   **Rule 1: ခွင့်ပြုချက်**
    *   **Action**: ALLOW
    *   **Protocol**: TCP
    *   **Source**: Office Public IP (203.0.113.50)
    *   **Destination**: File Server IP (198.51.100.25)
    *   **Destination Port**: 22 (SFTP uses SSH)
    *   **ရှင်းလင်းချက်**: ရုံး၏ Public IP မှသာ SFTP ဆာဗာသို့ ဝင်ရောက်ခွင့်ပေးသည်။

*   **Rule 2: ငြင်းပယ်ချက်**
    *   **Action**: DROP
    *   **Protocol**: TCP
    *   **Source**: Any
    *   **Destination**: File Server IP (198.51.100.25)
    *   **Destination Port**: 22 (SFTP uses SSH)
    *   **ရှင်းလင်းချက်**: ရုံး IP မှလွဲ၍ အခြားမည်သူမဆို ဝင်ရောက်ခွင့်ကို ငြင်းပယ်သည်။

**မှတ်ရန်**: Firewall သည် Rules စာရင်းကို **အပေါ်မှစ၍ အောက်သို့** ဖတ်သည်။ ပထမဆုံး ကိုက်ညီသော Rule ကို တွေ့သည်နှင့် ထို Rule ၏ Action (ALLOW/DENY) အတိုင်း ဆောင်ရွက်သည်။ ထို့ကြောင့် **အထူးသတ်မှတ်ချက် (Specific Rules)** များကို **အပေါ်ဆုံးတွင်** ထားရှိပြီး၊ **ယေဘူယျ သတ်မှတ်ချက် (General Rules)** များကို **အောက်ဆုံးတွင်** ထားရှိရပါမည်။

ဤနမူနာများသည် အခြေခံမျှသာဖြစ်ပြီး၊ လက်တွေ့တွင် ကွန်ယက်၏ လိုအပ်ချက်၊ ဝန်ဆောင်မှုများနှင့် လုံခြုံရေးပေါ်လစီအရ ပိုမိုရှုပ်ထွေးသော Rules များ ရေးဆွဲရနိုင်ပါသည်။