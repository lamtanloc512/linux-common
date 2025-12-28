# Linux Commands Hàng Ngày

## 1. File & Directory Operations

### Tạo và Xóa
```bash
# Tạo file
 touch file.txt
 touch file1.txt file2.txt

# Tạo directory
 mkdir mydir
 mkdir -p parent/child/grandchild

# Xóa file
 rm file.txt
 rm -f file.txt  # Force delete

# Xóa directory
 rmdir mydir  # Chỉ xóa empty directory
 rm -r mydir  # Xóa recursive
 rm -rf mydir  # Force recursive delete

# Xóa nhiều files
 rm file1.txt file2.txt
 rm *.log  # Xóa tất cả .log files
```

### Copy & Move
```bash
# Copy file
 cp file.txt /path/to/destination/
 cp file.txt newname.txt

# Copy directory
 cp -r mydir/ /path/to/destination/

# Copy và giữ permissions
 cp -p file.txt /path/

# Copy và không overwrite
 cp -n file.txt /path/

# Move file
 mv file.txt /path/to/destination/
 mv file.txt newname.txt

# Move directory
 mv mydir/ /path/to/destination/

# Move và overwrite
 mv -f file.txt /path/
```

### Search Files
```bash
# Tìm file theo tên
 find /path -name "filename.txt"
 find /path -name "*.log"

# Tìm file theo type
 find /path -type f  # Files
 find /path -type d  # Directories

# Tìm file theo size
 find /path -size +10M  # Lớn hơn 10MB
 find /path -size -1M   # Nhỏ hơn 1MB

# Tìm file theo modification time
 find /path -mtime -7   # Modified trong 7 ngày qua
 find /path -mtime +30  # Modified hơn 30 ngày

# Tìm file theo owner
 find /path -user username

# Tìm file theo permission
 find /path -perm 644

# Tìm và xóa
 find /path -name "*.tmp" -delete

# Tìm và execute command
 find /path -name "*.log" -exec rm {} \;
```

### File Content
```bash
# Xem file
 cat file.txt

# Xem với line numbers
 cat -n file.txt

# Xem từ cuối
 tail file.txt
 tail -n 10 file.txt  # 10 dòng cuối
 tail -f file.txt    # Follow file

# Xem từ đầu
 head file.txt
 head -n 5 file.txt  # 5 dòng đầu

# Xem với pagination
 less file.txt
 more file.txt

# Xem file lớn
 bat file.txt  # Syntax highlighting
 batcat file.txt

# Xem file binary
 hexdump -C file.bin
 xxd file.bin

# Xem file compressed
 zcat file.gz
 zless file.gz
```

### File Permissions
```bash
# Thay đổi permission
 chmod 644 file.txt
 chmod +x script.sh
 chmod -R 755 mydir/

# Thay đổi owner
 chown user:group file.txt
 chown -R user:group mydir/

# Thay đổi group
 chgrp group file.txt

# Xem permission
 ls -l file.txt
 stat file.txt

# Đặt sticky bit
 chmod +t /tmp

# Đặt setuid/setgid
 chmod u+s /usr/bin/program
 chmod g+s /usr/bin/program
```

---

## 2. Process Management

### Kill Process
```bash
# Kill bằng PID
 kill 1234
 kill -9 1234  # Force kill

# Kill bằng tên
 pkill -f "process_name"
 pkill -9 -f "process_name"

# Kill bằng tên chính xác
 killall nginx
 killall -9 nginx

# Kill port đang sử dụng
 sudo lsof -i :8080
 sudo kill -9 $(lsof -t -i :8080)

# Kill tất cả processes của user
 pkill -u username
 killall -u username
```

### Process Monitoring
```bash
# Liệt kê processes
 ps aux
 ps -ef

# Liệt kê processes của user
 ps -u username

# Liệt kê processes theo tên
 pgrep -l "nginx"

# Xem process tree
 pstree
 pstree -p

# Xem process chi tiết
 top
 htop

# Xem process với filter
 ps aux | grep nginx

# Xem process sử dụng nhiều CPU
 top -o %CPU

# Xem process sử dụng nhiều memory
 top -o %MEM
```

### Process Priority
```bash
# Thay đổi priority (nice)
 nice -n 10 command
 renice -n 10 -p 1234

# Thay đổi priority (real-time)
 chrt -r 1 -p 99 1234

# Xem priority
 ps -l -p 1234
```

---

## 3. System Management

### Restart Services
```bash
# Restart service
 sudo systemctl restart nginx
 sudo systemctl restart apache2

# Start/Stop service
 sudo systemctl start nginx
 sudo systemctl stop nginx

# Enable/Disable service
 sudo systemctl enable nginx
 sudo systemctl disable nginx

# Kiểm tra status
 sudo systemctl status nginx

# Reload service
 sudo systemctl reload nginx

# Restart tất cả services
 sudo systemctl restart --all
```

### System Information
```bash
# Xem OS information
 uname -a
 cat /etc/os-release
 lsb_release -a

# Xem kernel version
 uname -r

# Xem uptime
 uptime

# Xem CPU information
 lscpu
 cat /proc/cpuinfo

# Xem memory information
 free -h
 cat /proc/meminfo

# Xem disk information
 df -h
 lsblk
 fdisk -l

# Xem network information
 ip a
 ifconfig

# Xem hardware information
 lshw
 lspci
 lsusb
```

### System Monitoring
```bash
# Xem system load
 uptime
 cat /proc/loadavg

# Xem CPU usage
 mpstat -P ALL
 vmstat 1

# Xem memory usage
 free -h
 vmstat -s

# Xem disk usage
 df -h
 du -sh /path

# Xem network usage
 iftop
 nload

# Xem process usage
 htop
 atop
```

### System Control
```bash
# Reboot
 sudo reboot
 sudo shutdown -r now

# Shutdown
 sudo shutdown -h now
 sudo poweroff

# Sleep
 sudo systemctl suspend
 sudo systemctl hibernate

# Logout
 exit
 logout
```

---

## 4. Network Commands

### Network Configuration
```bash
# Xem IP address
 ip a
 ifconfig

# Xem routing table
 ip route
 route -n

# Xem DNS configuration
 cat /etc/resolv.conf

# Xem network interfaces
 ip link
 ifconfig -a

# Xem network statistics
 netstat -i
 ip -s link
```

### Network Testing
```bash
# Ping
 ping google.com
 ping -c 4 google.com

# Traceroute
 traceroute google.com
 mtr google.com

# DNS lookup
 nslookup google.com
 dig google.com
 host google.com

# Check port
 nc -zv google.com 80
 telnet google.com 80

# Check connectivity
 curl -I https://google.com
 wget -O /dev/null https://google.com
```

### Network Management
```bash
# Restart network
 sudo systemctl restart networking
 sudo systemctl restart NetworkManager

# Restart network interface
 sudo ifdown eth0
 sudo ifup eth0

# Thay đổi IP address
 sudo ip addr add 192.168.1.100/24 dev eth0

# Thay đổi default gateway
 sudo ip route add default via 192.168.1.1

# Thay đổi DNS
 sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

---

## 5. Package Management

### APT (Debian/Ubuntu)
```bash
# Update package list
 sudo apt update

# Upgrade packages
 sudo apt upgrade
 sudo apt full-upgrade

# Install package
 sudo apt install nginx

# Remove package
 sudo apt remove nginx
 sudo apt purge nginx

# Search package
 sudo apt search nginx

# Show package information
 sudo apt show nginx

# List installed packages
 sudo apt list --installed

# Clean cache
 sudo apt clean
 sudo apt autoclean
```

### YUM/DNF (CentOS/RHEL)
```bash
# Install package
 sudo yum install nginx
 sudo dnf install nginx

# Remove package
 sudo yum remove nginx
 sudo dnf remove nginx

# Update packages
 sudo yum update
 sudo dnf update

# Search package
 sudo yum search nginx
 sudo dnf search nginx

# List installed packages
 sudo yum list installed
 sudo dnf list installed

# Clean cache
 sudo yum clean all
 sudo dnf clean all
```

---

## 6. Text Processing

### Grep

#### Common Grep Flags Reference
| Flag | Description | Example |
|------|-------------|---------|
| `-i` | Case insensitive search (không phân biệt hoa thường) | `grep -i "error" file.txt` |
| `-n` | Show line numbers (hiển thị số dòng) | `grep -n "pattern" file.txt` |
| `-r` | Recursive search in directories (tìm kiếm đệ quy trong thư mục) | `grep -r "pattern" /path/` |
| `-v` | Invert match (hiển thị dòng KHÔNG khớp) | `grep -v "error" file.txt` |
| `-E` | Use extended regular expressions (sử dụng regex mở rộng) | `grep -E "pattern1\|pattern2" file.txt` |
| `-A N` | Show N lines after match (hiển thị N dòng sau kết quả khớp) | `grep -A 2 "error" file.txt` |
| `-B N` | Show N lines before match (hiển thị N dòng trước kết quả khớp) | `grep -B 2 "error" file.txt` |
| `-C N` | Show N lines around match (hiển thị N dòng xung quanh kết quả khớp) | `grep -C 2 "error" file.txt` |
| `-w` | Match whole words only (chỉ khớp từ hoàn chỉnh) | `grep -w "word" file.txt` |
| `-x` | Match entire lines only (chỉ khớp toàn bộ dòng) | `grep -x "exact line" file.txt` |
| `-l` | Show only filenames with matches (chỉ hiển thị tên file có kết quả) | `grep -l "pattern" *.txt` |
| `-L` | Show only filenames without matches (chỉ hiển thị tên file KHÔNG có kết quả) | `grep -L "pattern" *.txt` |
| `-c` | Count matches instead of showing them (đếm số kết quả thay vì hiển thị) | `grep -c "pattern" file.txt` |
| `-o` | Show only the matched part (chỉ hiển thị phần khớp) | `grep -o "pattern" file.txt` |
| `-f` | Read patterns from file (đọc pattern từ file) | `grep -f patterns.txt file.txt` |

#### Basic Grep Commands
```bash
# Tìm kiếm text cơ bản
grep "pattern" file.txt

# Tìm kiếm recursive trong thư mục (-r)
grep -r "pattern" /path/

# Tìm kiếm không phân biệt hoa thường (-i)
grep -i "pattern" file.txt

# Tìm kiếm với số dòng (-n)
grep -n "pattern" file.txt

# Tìm kiếm với context xung quanh (-C)
grep -C 5 "pattern" file.txt  # 5 dòng xung quanh
grep -A 5 "pattern" file.txt  # 5 dòng sau
grep -B 5 "pattern" file.txt  # 5 dòng trước

# Tìm kiếm với regex mở rộng (-E)
grep -E "pattern1|pattern2" file.txt
```

#### Practical Grep Examples

**Port Finding:**
```bash
# Tìm process đang listen trên port 80
ss -tuln | grep :80

# Kiểm tra port 8080 đang được sử dụng
netstat -tuln | grep :8080

# Tìm tất cả ports đang mở
ss -tuln | grep LISTEN
```

**Process Searches:**
```bash
# Tìm process nginx
ps aux | grep nginx

# Tìm process và loại bỏ grep khỏi kết quả (-v)
ps aux | grep -v grep | grep nginx

# Tìm process theo PID
ps aux | grep " 1234 "
```

**Log Analysis:**
```bash
# Tìm lỗi trong log nginx
grep "ERROR" /var/log/nginx/error.log

# Tìm failed login (không phân biệt hoa thường -i)
grep -i "failed" /var/log/auth.log

# Tìm lỗi trong 24h qua với timestamp
grep "$(date -d '1 day ago' +'%Y-%m-%d')" /var/log/syslog | grep -i error
```

**Pattern Matching với Regex (-E):**
```bash
# Tìm địa chỉ IP
grep -E "\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b" file.txt

# Tìm email addresses
grep -E "\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b" file.txt

# Tìm URLs
grep -E "https?://[^\s]+" file.txt

# Tìm số điện thoại Việt Nam
grep -E "\b(0[3|5|7|8|9])+([0-9]{8})\b" file.txt
```

**Command Output Filtering:**
```bash
# Tìm file .txt trong directory listing
ls -la | grep "\.txt$"

# Tìm disk usage cho các device /dev/sd
df -h | grep "/dev/sd"

# Tìm file lớn hơn 100MB
ls -lh | grep -E "[0-9]{3,}M|[0-9]+G"
```

**System Administration:**
```bash
# Tìm biến PATH trong environment
env | grep "PATH"

# Tìm cron job backup
crontab -l | grep "backup"

# Tìm service nginx
systemctl list-units --type=service | grep "nginx"

# Tìm package nginx đã cài đặt
apt list --installed | grep "nginx"
```

**Container Searches:**
```bash
# Tìm container nginx đang chạy
docker ps | grep "nginx"

# Tìm tất cả container nginx (bao gồm stopped)
docker ps -a | grep "nginx"

# Tìm image ubuntu
podman images | grep "ubuntu"
```

**Advanced Examples:**
```bash
# Tìm file chứa "TODO" và hiển thị tên file (-l)
grep -l "TODO" *.txt

# Đếm số dòng chứa "error" (-c)
grep -c "error" /var/log/syslog

# Tìm từ hoàn chỉnh "word" (-w)
grep -w "word" file.txt

# Tìm dòng chính xác (-x)
grep -x "exact line content" file.txt

# Tìm và hiển thị chỉ phần khớp (-o)
echo "The quick brown fox" | grep -o "quick\|brown\|fox"

# Tìm với pattern từ file (-f)
echo -e "pattern1\npattern2" > patterns.txt
grep -f patterns.txt file.txt

# Tìm file không chứa pattern (-L)
grep -L "pattern" *.txt
```

### Sed
```bash
# Thay thế text
 sed 's/old/new/g' file.txt

# Thay thế và save
 sed -i 's/old/new/g' file.txt

# Thay thế với backup
 sed -i.bak 's/old/new/g' file.txt

# Thay thế từ dòng 1 đến 10
 sed '1,10s/old/new/g' file.txt

# Thay thế với regex
 sed -E 's/pattern1|pattern2/new/g' file.txt
```

### Awk
```bash
# Trích xuất cột
 awk '{print $1}' file.txt

# Trích xuất cột với delimiter
 awk -F',' '{print $1}' file.csv

# Trích xuất cột với điều kiện
 awk '$1 == "value" {print $2}' file.txt

# Trích xuất cột với regex
 awk '/pattern/ {print $1}' file.txt

# Trích xuất cột với format
 awk '{printf "%s %s\n", $1, $2}' file.txt
```

---

## 7. Common Flag Patterns Across Linux Commands

Nhiều flags trong Linux có patterns tương tự nhau, mặc dù không phải tất cả commands đều dùng cùng cú pháp. Dưới đây là các patterns phổ biến:

### Long Options vs Short Options
Hầu hết commands hỗ trợ cả dạng ngắn (`-l`) và dài (`--long-option`):
```bash
ls -l          # Short option
ls --long      # Long option

ps -aux        # Short options combined
ps --all --user --extra  # Long options
```

### Common Flag Patterns

| Pattern | Ý nghĩa | Ví dụ trong các commands |
|---------|---------|--------------------------|
| `-h, --help` | Hiển thị help | `grep --help`, `ls --help`, `ps --help` |
| `-v, --verbose` | Verbose output (chi tiết hơn) | `cp -v`, `mv -v`, `rm -v`, `tar -v` |
| `-f, --force` | Force action (bắt buộc) | `rm -f`, `cp -f`, `mv -f`, `ln -f` |
| `-i, --interactive` | Interactive mode (hỏi trước khi thực hiện) | `rm -i`, `cp -i`, `mv -i` |
| `-r, -R, --recursive` | Recursive operation (đệ quy) | `cp -r`, `rm -r`, `grep -r`, `find -R` |
| `-a, --all` | Show all (hiển thị tất cả) | `ls -a`, `ps -a`, `find -a` |
| `-l, --long` | Long format (định dạng dài) | `ls -l`, `ps -l` |
| `-n, --number` | Show numbers (hiển thị số) | `grep -n`, `cat -n`, `head -n` |
| `-c, --count` | Count instead of show (đếm thay vì hiển thị) | `grep -c`, `wc -c` |
| `-o, --output` | Specify output (chỉ định output) | `grep -o`, `sort -o` |
| `-s, --silent` | Silent mode (im lặng) | `wget -s`, `curl -s` |
| `-q, --quiet` | Quiet mode (yên tĩnh) | `tar -q`, `rsync -q` |
| `-d, --directory` | Directory operation (thao tác với thư mục) | `ls -d`, `cp -d` |
| `-p, --preserve` | Preserve attributes (giữ nguyên thuộc tính) | `cp -p`, `tar -p` |
| `-t, --time` | Time-related (liên quan đến thời gian) | `ls -t`, `ps -t`, `find -mtime` |
| `-u, --user` | User-related (liên quan đến user) | `ps -u`, `find -user` |
| `-g, --group` | Group-related (liên quan đến group) | `ls -g`, `find -group` |
| `-m, --mode` | Permission mode (quyền truy cập) | `chmod -m`, `find -perm` |
| `-x, --exclude` | Exclude something (loại trừ) | `grep -x`, `tar --exclude` |
| `-I, --include` | Include something (bao gồm) | `tar --include` |

### Examples of Similar Flags Across Commands

#### Verbose Mode (`-v, --verbose`)
```bash
# Copy với verbose
cp -v file.txt dest/

# Move với verbose
mv -v file.txt dest/

# Remove với verbose
rm -v file.txt

# Tar với verbose
tar -cvf archive.tar dir/  # -v shows files being processed
```

#### Recursive Mode (`-r, -R, --recursive`)
```bash
# Copy recursive
cp -r src/ dest/

# Remove recursive
rm -r dir/

# Grep recursive
grep -r "pattern" /path/

# Find recursive (default behavior)
find /path -name "*.txt"
```

#### Force Mode (`-f, --force`)
```bash
# Remove force
rm -f file.txt

# Copy force (overwrite)
cp -f src.txt dest.txt

# Move force
mv -f src.txt dest.txt

# Link force
ln -f target linkname
```

#### Interactive Mode (`-i, --interactive`)
```bash
# Remove interactive (hỏi trước khi xóa)
rm -i file.txt

# Copy interactive
cp -i src.txt dest.txt

# Move interactive
mv -i src.txt dest.txt
```

#### Long Format (`-l, --long`)
```bash
# ls long format
ls -l

# ps long format
ps -l

# find với long listing
find . -ls
```

#### All Items (`-a, --all`)
```bash
# ls show all files
ls -a

# ps show all processes
ps -a

# find all files (including hidden)
find . -name ".*"
```

#### Number-related (`-n, --number`)
```bash
# grep show line numbers
grep -n "pattern" file.txt

# cat show line numbers
cat -n file.txt

# head show first N lines
head -n 5 file.txt

# tail show last N lines
tail -n 10 file.txt
```

### Commands with Unique but Similar Concepts

#### Sort Command (có flags tương tự grep)
```bash
# Sort với ignore case (-i)
sort -i file.txt

# Sort numeric (-n)
sort -n numbers.txt

# Sort reverse (-r)
sort -r file.txt

# Sort unique (-u)
sort -u file.txt
```

#### Find Command (có logic tương tự grep)
```bash
# Find theo tên (tương tự grep pattern)
find /path -name "*.txt"

# Find theo type
find /path -type f  # files
find /path -type d  # directories

# Find theo size
find /path -size +10M

# Find theo time
find /path -mtime -7  # modified in last 7 days
```

#### Sed Command (text processing như grep)
```bash
# Sed substitute (thay thế như grep -o)
sed 's/old/new/g' file.txt

# Sed với regex (-E)
sed -E 's/pattern/replacement/g' file.txt

# Sed in-place (-i)
sed -i 's/old/new/g' file.txt
```

### Note về Consistency
- **Không phải tất cả flags đều giống nhau**: Ví dụ `-r` trong `grep` là recursive, nhưng trong `sort` có thể là reverse
- **GNU tools thường consistent hơn**: Các tool GNU (như grep, ls, cp) thường có long options (`--option`) và short options (`-o`)
- **POSIX standard**: Nhiều commands tuân theo POSIX standard nên có flags tương tự
- **Check man pages**: Luôn dùng `man command` hoặc `command --help` để xem flags chính xác

---

## 8. Compression & Archiving

### Tar
```bash
# Tạo archive
 tar -cvf archive.tar /path/to/dir/

# Tạo archive với compression
 tar -czvf archive.tar.gz /path/to/dir/
 tar -cjvf archive.tar.bz2 /path/to/dir/
 tar -cJvf archive.tar.xz /path/to/dir/

# Extract archive
 tar -xvf archive.tar
 tar -xzvf archive.tar.gz
 tar -xjvf archive.tar.bz2
 tar -xJvf archive.tar.xz

# Extract vào directory cụ thể
 tar -xvf archive.tar -C /path/to/dir/

# List contents
 tar -tvf archive.tar
```

### Zip
```bash
# Tạo zip
 zip archive.zip file1.txt file2.txt
 zip -r archive.zip /path/to/dir/

# Extract zip
 unzip archive.zip
 unzip archive.zip -d /path/to/dir/

# List contents
 unzip -l archive.zip
```

### Gzip
```bash
# Compress file
 gzip file.txt

# Decompress file
 gunzip file.txt.gz
 gzip -d file.txt.gz

# Compress với level
 gzip -9 file.txt  # Maximum compression
```

---

## 8. Disk & Filesystem

### Disk Usage
```bash
# Xem disk usage
 df -h
 df -hT

# Xem directory size
 du -sh /path/to/dir/
 du -sh *

# Xem file size
 du -sh file.txt

# Xem top 10 largest files
 du -ah /path | sort -rh | head -n 10

# Xem top 10 largest directories
 du -h --max-depth=1 /path | sort -rh | head -n 10
```

### Filesystem
```bash
# Format disk
 sudo mkfs.ext4 /dev/sdX1

# Mount disk
 sudo mount /dev/sdX1 /mnt

# Unmount disk
 sudo umount /mnt

# Check filesystem
 sudo fsck /dev/sdX1

# Resize filesystem
 sudo resize2fs /dev/sdX1
```

---

## 9. User & Group Management

### User Management
```bash
# Tạo user
 sudo adduser username
 sudo useradd -m username

# Xóa user
 sudo deluser username
 sudo userdel -r username

# Thay đổi password
 sudo passwd username

# Thay đổi user information
 sudo chfn username

# Thay đổi shell
 sudo chsh -s /bin/bash username

# Xem user information
 id username
 finger username
```

### Group Management
```bash
# Tạo group
 sudo addgroup groupname
 sudo groupadd groupname

# Xóa group
 sudo delgroup groupname
 sudo groupdel groupname

# Thêm user vào group
 sudo usermod -aG groupname username

# Xóa user khỏi group
 sudo gpasswd -d username groupname

# Xem group information
 groups username
 getent group groupname
```

---

## 10. Cron Jobs

### Cron Management
```bash
# Xem cron jobs
 crontab -l

# Edit cron jobs
 crontab -e

# Xóa cron jobs
 crontab -r

# Xem cron logs
 grep CRON /var/log/syslog

# Xem cron jobs của user khác
 sudo crontab -u username -l
```

### Cron Examples
```bash
# Chạy mỗi phút
 * * * * * /path/to/command

# Chạy mỗi 5 phút
 */5 * * * * /path/to/command

# Chạy mỗi giờ
 0 * * * * /path/to/command

# Chạy mỗi ngày lúc 2h sáng
 0 2 * * * /path/to/command

# Chạy mỗi tuần vào chủ nhật
 0 2 * * 0 /path/to/command

# Chạy mỗi tháng vào ngày 1
 0 2 1 * * /path/to/command

# Chạy mỗi năm vào ngày 1 tháng 1
 0 2 1 1 * /path/to/command
```

---

## 11. Logs & Debugging

### Log Management
```bash
# Xem logs
 tail -f /var/log/syslog
 tail -f /var/log/nginx/error.log

# Tìm kiếm trong logs
 grep "error" /var/log/syslog

# Xem logs với journalctl
 journalctl -u nginx
 journalctl -u nginx -f

# Xem logs với filter
 journalctl -u nginx --since "2024-01-01"
 journalctl -u nginx --since "1 hour ago"

# Xem logs với priority
 journalctl -p err
 journalctl -p warning
```

### Debugging
```bash
# Debug với strace
 strace -p 1234
 strace -f -o debug.log command

# Debug với ltrace
 ltrace -p 1234
 ltrace -f -o debug.log command

# Debug với gdb
 gdb --pid 1234
 gdb command

# Debug với valgrind
 valgrind --leak-check=full command
```

---

## 12. Useful One-liners

```bash
# Tìm và kill process
 pkill -f "process_name"

# Tìm và xóa files
 find /path -name "*.tmp" -delete

# Tìm và thay thế text
 find /path -name "*.txt" -exec sed -i 's/old/new/g' {} \;

# Copy và backup
 cp file.txt file.txt.bak

# Move và backup
 mv file.txt file.txt.bak

# Restart service và check status
 sudo systemctl restart nginx && sudo systemctl status nginx

# Kill port và restart service
 sudo kill -9 $(lsof -t -i :8080) && sudo systemctl restart nginx

# Tìm và xóa empty directories
 find /path -type d -empty -delete

# Tìm và xóa duplicate files
 find /path -type f -exec md5sum {} \; | sort | uniq -w32 -d | awk '{print $2}' | xargs rm

# Tìm và xóa files lớn hơn 100MB
 find /path -type f -size +100M -delete

# Tìm và xóa files cũ hơn 30 ngày
 find /path -type f -mtime +30 -delete
```

---

## Tài liệu Tham Khảo

- [Linux Command Line Basics](https://ubuntu.com/tutorials/command-line-for-beginners)
- [Linux Documentation Project](https://tldp.org/)
- [GNU Coreutils](https://www.gnu.org/software/coreutils/)
- [Linux Man Pages](https://man7.org/linux/man-pages/)
