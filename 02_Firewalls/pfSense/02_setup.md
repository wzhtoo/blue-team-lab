## **VirtualBox မှာ pfSense Setup အဆင့်ဆင့်**

### **၁။ ပြင်ဆင်မှုအဆင့်**
1. **pfSense ISO download** - [pfSense.org](https://www.pfsense.org/download/) မှာ AMD64 (64-bit) ISO ကိုဒေါင်းလုပ်
2. **VirtualBox ကို update** - နောက်ဆုံးဗားရှင်းရအောင်လုပ်ပါ

### **၂။ VM ဖန်တီးခြင်း**
```
Name: pfSense
Type: BSD
Version: FreeBSD (64-bit)
Memory: 1024 MB (minimum) - 2048 MB ပေးရင်ပိုကောင်း
Hard disk: 10 GB (VDI, Dynamically allocated)
```

### **၃။ Network Settings (အရေးကြီးဆုံးအပိုင်း)**
VirtualBox မှာ **Network Adapter ၂ ခု** လိုအပ်ပါတယ်:

**Adapter 1:**
- **Attached to:** Bridged Adapter
- **Name:** ခင်ဗျား၏ မူလ network adapter (WAN အတွက်)
- **Promiscuous Mode:** Allow All

**Adapter 2:**
- **Attached to:** Internal Network
- **Name:** intnet (သို့မဟုတ်) ကိုယ်ပိုင် network name (LAN အတွက်)
- **Promiscuous Mode:** Allow All

### **၄။ Installation Process**
1. ISO ကို VM ထဲမှာ boot
2. `Accept` > `Quick/Easy Install` ကိုရွေး
3. Reboot after installation
4. **Assign Interfaces** - VLANs configure မလုပ်ချင်ရင် `n` နှိပ်

### **၅။ Interface Assignment**
```
Should VLANs be set up now? [y|n]: n

Enter the WAN interface name: em0 (သို့မဟုတ် ပေါ်လာတဲ့ interface name)
Enter the LAN interface name: em1 (သို့မဟုတ် ဒုတိယ interface name)
```

### **၆။ Access pfSense Web Interface**
Installation ပြီးရင်:
1. LAN interface က `192.168.1.1` ကို DHCP ကပေးပါလိမ့်မယ်
2. Host PC ကနေ browser ဖွင့်ပြီး `https://192.168.1.1` ကိုသွားပါ
3. Username: `admin` | Password: `pfsense`
---

**သတိပြုရန်:** VirtualBox မှာ pfSense setup လုပ်ရင် **Network Adapter ၂ ခု** မထားမိရင် အလုပ်မလုပ်ပါဘူး။ WAN (အပြင်ကို ချိတ်ဆက်ဖို့) နဲ့ LAN (အတွင်းကို ချိတ်ဆက်ဖို့) လိုအပ်ပါတယ်။

