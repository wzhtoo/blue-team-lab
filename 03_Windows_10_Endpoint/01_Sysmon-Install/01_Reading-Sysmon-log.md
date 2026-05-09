### အဆင့် (၁) - Wazuh Agent အား Sysmon Log ဖတ်ရှုရန် လမ်းညွှန်ခြင်း

Sysmon ကို Install လုပ်ပြီးရုံနဲ့ Wazuh က အလိုအလျောက် သိမှာမဟုတ်ပါဘူး။ Wazuh Agent ကို Windows ရဲ့ **Event Viewer** ထဲက Sysmon logs တွေကို ဖတ်ပါလို့ အရင်ခိုင်းရပါမယ်။

**လုပ်ဆောင်ရန် အဆင့်များ:**

1. **window_10VM** ထဲမှာ **Notepad** ကို **Administrator** အဖြစ် ဖွင့်ပါ။
2. `C:\Program Files (x86)\ossec-agent\ossec.conf` ဖိုင်ကို ဖွင့်ပါ။
3. ဖိုင်ရဲ့ အောက်ဆုံးနား (သို့မဟုတ် `<ossec_config>` tag မပိတ်ခင်) မှာ အောက်ပါ XML စာသားကို ကူးထည့်ပေးပါ-

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventlog</log_format>
</localfile>

```

4. ဖိုင်ကို **Save** လုပ်ပြီး ပိတ်လိုက်ပါ။

---

**ဘာကြောင့် ဒါကို လုပ်ရတာလဲ?**
Windows ရဲ့ Event Viewer ထဲမှာ Sysmon logs တွေက သီးသန့်နေရာတစ်ခု (`Microsoft-Windows-Sysmon/Operational`) မှာ ရှိနေတာဖြစ်လို့၊ Wazuh Agent ကို အဲဒီလမ်းကြောင်းက data တွေကို ယူပြီး Manager ဆီ ပို့ပေးဖို့ လမ်းကြောင်းပြပေးရတာ ဖြစ်ပါတယ်။
