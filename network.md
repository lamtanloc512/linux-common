# Các lệnh Network phổ biến cho VPS

## 1. SSH Commands

### SSH Connection
```bash
# Kết nối SSH cơ bản
ssh user@VPS_IP

# Kết nối với port khác
ssh -p 2222 user@VPS_IP

# Kết nối với private key
ssh -i /path/to/private_key user@VPS_IP

# Kết nối và giữ alive
ssh -o ServerAliveInterval=60 user@VPS_IP
```

### SSH Tunneling

#### Local Port Forwarding (SSH Tunnel)
```bash
# Forward port từ VPS về máy local
ssh -L [local-port]:localhost:[vps-port] user@VPS_IP

# Ví dụ: Access MySQL trên VPS qua port 3306
ssh -L 3306:localhost:3306 user@VPS_IP

# Ví dụ: Access web server trên VPS port 8080
ssh -L 8080:localhost:8080 user@VPS_IP

# Forward port với dynamic SOCKS proxy
ssh -D 1080 user@VPS_IP

# Forward với background mode
ssh -f -N -L 8080:localhost:8080 user@VPS_IP
```

#### Remote Port Forwarding
```bash
# Forward port từ local ra VPS (cho phép external access)
ssh -R [vps-port]:localhost:[local-port] user@VPS_IP

# Ví dụ: Expose local web server ra internet qua VPS
ssh -R 80:localhost:8080 user@VPS_IP

# Remote forward với bind address
ssh -R 0.0.0.0:8080:localhost:8080 user@VPS_IP
```

#### SSH Jump Host
```bash
# Kết nối qua intermediate server
ssh -J user@jump-server.com user@target-server.com

# Kết nối qua nhiều jumps
ssh -J user@jum1.com,user@jum2.com user@target.com

# Sử dụng config file
ssh -J user@jump-server.com user@target-server.com
```

### SSH Config
```bash
# Thêm vào ~/.ssh/config
Host vps
    HostName VPS_IP
    User username
    Port 22
    IdentityFile ~/.ssh/id_rsa
    LocalForward 3306 localhost:3306
    LocalForward 8080 localhost:8080

Host jump
    HostName jump-server.com
    User admin

Host prod
    HostName prod-server.com
    User deploy
    ProxyJump jump
```

---

## 2. SCP Commands

```bash
# Copy từ local lên VPS
scp file.txt user@VPS_IP:/path/to/destination/

# Copy từ VPS về local
scp user@VPS_IP:/path/to/file.txt ./

# Copy directory
scp -r /local/directory user@VPS_IP:/remote/directory/

# Copy với port khác
scp -P 2222 file.txt user@VPS_IP:/path/

# Copy với compression
scp -C file.txt user@VPS_IP:/path/

# Copy và giữ permissions
scp -p file.txt user@VPS_IP:/path/
```

---

## 3. Rsync Commands

```bash
# Sync từ local lên VPS
rsync -avz /local/dir/ user@VPS_IP:/remote/dir/

# Sync với exclude
rsync -avz --exclude='node_modules' --exclude='.git' /local/ user@VPS_IP:/remote/

# Sync với delete (mirror)
rsync -avz --delete /local/ user@VPS_IP:/remote/

# Sync với progress
rsync -avzP /local/ user@VPS_IP:/remote/

# Sync qua SSH với port khác
rsync -avz -e 'ssh -p 2222' /local/ user@VPS_IP:/remote/
```

---

## 4. Port Management

### Kiểm tra ports đang listen
```bash
# Liệt kê tất cả ports đang listen
ss -tuln

# Với process name
ss -tuln -p

# Liệt kê theo protocol
ss -tln  # TCP
ss -uln  # UDP

# Hiển thị chi tiết
ss -ltun

# Với PID
ss -tlnp
```

### Kiểm tra port đang sử dụng
```bash
# Kiểm tra port cụ thể
ss -tlnp | grep 80

# Sử dụng lsof
lsof -i :80

# Sử dụng netstat
netstat -tuln | grep 80

# Kiểm tra port từ xa
nc -zv VPS_IP 80
nc -zv VPS_IP 22 80 443

# Port scan nhanh
nc -zv -w 3 VPS_IP 1-1000
```

### Mở port (Firewall)

#### UFW (Ubuntu)
```bash
# Mở port
sudo ufw allow 80
sudo ufw allow 8080/tcp

# Mở port từ IP cụ thể
sudo ufw allow from 192.168.1.100 to any port 22

# Mở port range
sudo ufw allow 8000:9000/tcp

# Xóa rule
sudo ufw delete allow 80

# Enable/disable
sudo ufw enable
sudo ufw disable

# Kiểm tra status
sudo ufw status
sudo ufw status numbered
```

#### firewalld (CentOS/RHEL)
```bash
# Mở port
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=8080/tcp

# Reload firewall
sudo firewall-cmd --reload

# Kiểm tra ports đang mở
sudo firewall-cmd --list-ports

# Mở service
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https

# Xóa port
sudo firewall-cmd --permanent --remove-port=80/tcp
```

#### iptables
```bash
# Mở port
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Mở từ IP cụ thể
sudo iptables -A INPUT -s 192.168.1.100 -p tcp --dport 22 -j ACCEPT

# Xóa rule
sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT

# Liệt kê rules
sudo iptables -L -n -v

# Lưu rules (Ubuntu)
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

---

## 5. Network Diagnostics

### Ping & Traceroute
```bash
# Ping
ping VPS_IP
ping -c 4 VPS_IP
ping -i 0.5 VPS_IP

# Traceroute
traceroute VPS_IP

# MTR (My Traceroute)
mtr VPS_IP

# Tracepath
tracepath VPS_IP
```

### DNS
```bash
# Kiểm tra DNS resolution
dig domain.com
dig domain.com +short
nslookup domain.com
host domain.com

# DNS lookup chi tiết
dig domain.com ANY +noall +answer

# Reverse DNS
dig -x VPS_IP
nslookup VPS_IP
```

### Connectivity
```bash
# Kiểm tra kết nối TCP
nc -zv VPS_IP 22
nc -zv VPS_IP 80 443

# Kiểm tra với timeout
nc -w 3 -zv VPS_IP 80

# Telnet
telnet VPS_IP 22
telnet VPS_IP 80

# Curl
curl -I https://domain.com
curl -v https://domain.com

# wget
wget -O /dev/null https://domain.com
```

### Bandwidth Speed Test
```bash
# Speed test
curl -s https://raw.githubusercontent.com/sivel/speedtest-cli/master/speedtest.py | python3 -

# Hoặc
apt install speedtest-cli
speedtest-cli

# Kiểm tra bandwidth với iperf3
# Trên VPS (server)
iperf3 -s

# Trên local (client)
iperf3 -c VPS_IP
iperf3 -c VPS_IP -t 30 -P 4
```

---

## 6. Proxy & SOCKS

### SOCKS Proxy với SSH
```bash
# Tạo SOCKS proxy
ssh -D 1080 -N user@VPS_IP

# SOCKS proxy với background
ssh -f -N -D 1080 user@VPS_IP

# Dynamic forwarding với jump
ssh -J user@jump-server.com -D 1080 user@target-server.com
```

### HTTP Proxy
```bash
# Tạo HTTP proxy tunnel qua SSH
ssh -L 8888:localhost:8888 user@VPS_IP

# Sử dụng proxychains
# Cài đặt: sudo apt install proxychains
# Cấu hình: /etc/proxychains.conf
proxychains curl https://domain.com

# SSH qua HTTP proxy
ssh -o "ProxyCommand nc -X connect -x proxy-server:8080 %h %p" user@VPS_IP
```

### VPN over SSH
```bash
# TUN/TAP VPN (yêu cầu root)
ssh -w 0:0 user@VPS_IP

# SSHuttle (dễ sử dụng hơn)
# Cài đặt: pip3 install sshuttle
sshuttle -r user@VPS_IP 0.0.0.0/0

# Chỉ route một subnet
sshuttle -r user@VPS_IP 192.168.1.0/24
```

---

## 7. Firewall & Security

### Kiểm tra kết nối đang mở
```bash
# Liệt kê kết nối
ss -s
ss -ant

# Liệt kê connections theo state
ss -tan state established

# Liệt kê theo process
ss -tnp | grep sshd

# Kiểm tra whois
whois VPS_IP

# Kiểm tra ports đang mở từ bên ngoài
nmap -sS -p- VPS_IP
```

### SSH Security
```bash
# Kiểm tra SSH versions đang chạy
ssh -V

# Kiểm tra failed login attempts
grep "Failed password" /var/log/auth.log

# Kiểm tra successful logins
grep "Accepted" /var/log/auth.log

# Xem SSH connections hiện tại
who
w
last

# Kill SSH sessions
pkill -KILL -u username
```

---

## 8. Monitoring

### Network Traffic
```bash
# Real-time traffic
iftop
nload
nethogs

# Traffic per process
nethogs eth0

# Packet capture
tcpdump -i eth0 -w capture.pcap
tcpdump -i eth0 host VPS_IP

# Xem capture
tcpdump -r capture.pcap
tcpdump -r capture.pcap port 80
```

### Bandwidth Usage
```bash
# vnStat - traffic monitoring
vnstat
vnstat -h  # hourly
vnstat -d  # daily
vnstat -m  # monthly

# bmon - bandwidth monitor
bmon
```

---

## 9. VPN (WireGuard)

### WireGuard Server
```bash
# Cài đặt
sudo apt install wireguard

# Tạo keys
wg genkey | tee privatekey | wg pubkey > publickey

# Cấu hình /etc/wireguard/wg0.conf
[Interface]
Address = 10.0.0.1/24
ListenPort = 51820
PrivateKey = YOUR_PRIVATE_KEY
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT

[Peer]
PublicKey = CLIENT_PUBLIC_KEY
AllowedIPs = 10.0.0.2/32

# Enable
sudo wg-quick up wg0

# Kiểm tra
sudo wg show
```

### WireGuard Client
```bash
# Cấu hình /etc/wireguard/wg0.conf
[Interface]
Address = 10.0.0.2/24
PrivateKey = CLIENT_PRIVATE_KEY
DNS = 1.1.1.1

[Peer]
PublicKey = SERVER_PUBLIC_KEY
Endpoint = VPS_IP:51820
AllowedIPs = 0.0.0.0/0

# Connect
sudo wg-quick up wg0
```

---

## 10. Useful One-liners

```bash
# Copy SSH key lên VPS
ssh-copy-id user@VPS_IP

# Chạy command từ xa
ssh user@VPS_IP "df -h"

# Transfer và execute script
ssh user@VPS_IP "curl -s https://script.sh | bash"

# Keep SSH alive
ssh -o ServerAliveInterval=60 user@VPS_IP

# Mở nhiều tabs với tmux
ssh user@VPS_IP -t 'tmux new-session -A -s main'

# Sync và restart service
rsync -avz /local/ user@VPS_IP:/remote/ && ssh user@VPS_IP "sudo systemctl restart myservice"
```

---

## Tài liệu Tham Khảo

- [SSH Documentation](https://www.ssh.com/academy/ssh)
- [OpenSSH Manual](https://www.openssh.com/manual.html)
- [DigitalOcean SSH Tutorials](https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server)
- [WireGuard Documentation](https://www.wireguard.com/)
