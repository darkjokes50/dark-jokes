# dark-jokes
اداه اختبار اختراق شبكه
#!/bin/bash

# ألوان للنص
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
PURPLE='\033[0;35m'
CYAN='\033[0;36m'
NC='\033[0m' # No Color

clear
echo -e "${RED}
   _          _        
  |  __ \        | |   / /   \ \\
  | |  | |  _| | | | _| | _  _ __
  | |  | |/ _' | |/ _' |/ _ \ |/ _ \| '_ \\
  | || | (_| | | (_| |  / | (_) | | | |
  |_/ \,_|_|\,_|\_|_|\_/|_| |_|
${NC}"

echo -e "${GREEN} [+] Dark Jokes - Network Pentest Tool [+] ${NC}"
echo -e "${CYAN}           Created with ❤️ for Ethical Hackers ${NC}"
echo -e "============================================="

# دالة لفحص الأجهزة على الشبكة
scan_devices() {
    echo -e "${YELLOW}[*] Scanning for connected devices...${NC}"
    if ! command -v nmap &> /dev/null; then
        echo -e "${RED}[-] Nmap is not installed! Installing...${NC}"
        pkg install nmap -y
    fi
    nmap -sn 192.168.1.0/24
}

# دالة لفحص المنافذ المفتوحة
scan_ports() {
    read -p "Enter target IP: " target
    echo -e "${YELLOW}[*] Scanning ports on $target...${NC}"
    nmap -p 1-1000 -T4 $target
}

# دالة لاختبار ثغرات الويب
web_vuln_scan() {
    read -p "Enter target URL (e.g., http://example.com): " url
    echo -e "${YELLOW}[*] Scanning for web vulnerabilities...${NC}"
    if ! command -v nikto &> /dev/null; then
        echo -e "${RED}[-] Nikto is not installed! Install with: pkg install nikto -y${NC}"
        return
    fi
    nikto -h $url
}

# دالة لاختبار كلمات مرور SSH
ssh_attack() {
    read -p "Enter target IP: " target
    read -p "Enter username: " user
    read -p "Enter wordlist path: " wordlist
    echo -e "${YELLOW}[*] Launching SSH brute force...${NC}"
    if ! command -v hydra &> /dev/null; then
        echo -e "${RED}[-] Hydra is not installed! Install with: pkg install hydra -y${NC}"
        return
    fi
    hydra -l $user -P $wordlist $target ssh -t 4
}

# دالة لعرض نكات عشوائية
dark_jokes_mode() {
    jokes=(
        "Why do hackers love nature? Because they love brute-forcing trees."
        "Why did the hacker go broke? Because he lost his private key!"
        "Why do programmers prefer dark mode? Because light attracts bugs."
    )
    echo -e "${PURPLE}[🤖] ${jokes[$((RANDOM % ${#jokes[@]}))]}${NC}"
}

# القائمة الرئيسية
main_menu() {
    echo -e "\n${BLUE}1. Scan Network Devices"
    echo "2. Scan Open Ports"
    echo "3. Web Vulnerability Scan"
    echo "4. SSH Brute Force"
    echo "5. Dark Jokes Mode"
    echo "6. Exit${NC}"
    read -p "Choose an option: " option

    case $option in
        1) scan_devices ;;
        2) scan_ports ;;
        3) web_vuln_scan ;;
        4) ssh_attack ;;
        5) dark_jokes_mode ;;
        6) exit 0 ;;
        *) echo -e "${RED}[-] Invalid option!${NC}" ;;
    esac
}

# تشغيل الأداة
while true; do
    main_menu
done
