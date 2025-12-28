# Podman CLI Reference Guide

## 1. Image Management

### Pull Image
```bash
# Pull từ Docker Hub
podman pull ubuntu:latest

# Pull từ registry cụ thể
podman pull docker.io/library/ubuntu:22.04
podman pull quay.io/redhat/ubi8:latest

# Pull với nhiều tag
podman pull docker.io/library/nginx:1.25-alpine
```

### List Images
```bash
# Liệt kê tất cả images
podman images

# Liệt kê images bao gồm intermediate images
podman images -a

# Hiển thị chỉ image ID
podman images -q

# Hiển thị image ID và kích thước
podman images --format "table {{.ID}}\t{{.Size}}\t{{.Repository}}"
```

### Remove Images
```bash
# Xóa một image
podman rmi ubuntu:latest

# Xóa nhiều images
podman rmi ubuntu:latest nginx:latest

# Xóa image đang được sử dụng bởi container
podman rmi -f ubuntu:latest

# Xóa tất cả images không sử dụng
podman image prune -a

# Xóa tất cả images
podman rmi $(podman images -q)
```

### Search Images
```bash
# Tìm kiếm image trên Docker Hub
podman search nginx

# Tìm kiếm với filter
podman search --filter=stars=50 nginx

# Tìm kiếm official images only
podman search --filter=is-official=true nginx
```

---

## 2. Container Management

### Run Container
```bash
# Chạy container với interactive mode
podman run -it --name mycontainer ubuntu:latest /bin/bash

# Chạy container ở chế độ detached
podman run -d --name webserver nginx:latest

# Chạy container với port mapping
podman run -d -p 8080:80 --name web nginx:latest

# Chạy container với volume mount
podman run -v /host/path:/container/path --name mydata ubuntu:latest

# Chạy container với environment variables
podman run -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=mydb mysql:latest

# Chạy container với restart policy
podman run -d --restart=always --name web nginx:latest

# Chạy container với memory limit
podman run -d -m 512m --name limited-container nginx:latest

# Chạy container và tự động xóa khi exit
podman run --rm --name temp-container ubuntu:latest echo "Hello"
```

### List Containers
```bash
# Liệt kê containers đang chạy
podman ps

# Liệt kê tất cả containers (bao gồm stopped)
podman ps -a

# Hiển thị container ID
podman ps -q

# Hiển thị containers với format tùy chỉnh
podman ps --format "table {{.ID}}\t{{.Image}}\t{{.Status}}\t{{.Names}}"
```

### Start/Stop/Restart Containers
```bash
# Bắt đầu một container
podman start mycontainer

# Dừng một container
podman stop mycontainer

# Restart một container
podman restart mycontainer

# Dừng tất cả containers đang chạy
podman stop $(podman ps -q)

# Bắt đầu tất cả containers
podman start $(podman ps -aq)

# Kill container (dừng ngay lập tức)
podman kill mycontainer
```

### Remove Containers
```bash
# Xóa một container
podman rm mycontainer

# Xóa nhiều containers
podman rm container1 container2 container3

# Xóa tất cả stopped containers
podman container prune -f

# Xóa container đang chạy (force)
podman rm -f mycontainer

# Xóa tất cả containers
podman rm $(podman ps -aq)
```

### Container Logs
```bash
# Xem logs của container
podman logs mycontainer

# Xem logs với follow mode
podman logs -f mycontainer

# Xem logs với timestamps
podman logs -t mycontainer

# Xem số dòng logs nhất định
podman logs --tail 100 mycontainer
```

### Execute Command in Container
```bash
# Chạy command trong container đang chạy
podman exec mycontainer ls /app

# Chạy command với interactive mode
podman exec -it mycontainer /bin/bash

# Chạy command với user cụ thể
podman exec -u root mycontainer ls /root

# Chạy một lệnh và exit
podman exec mycontainer cat /etc/hostname
```

### Container Inspect
```bash
# Kiểm tra thông tin chi tiết container
podman inspect mycontainer

# Kiểm tra với format cụ thể
podman inspect --format='{{.State.Status}}' mycontainer
podman inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer
```

### Container Stats
```bash
# Xem thống kê tài nguyên container
podman stats mycontainer

# Xem thống kê tất cả containers
podman stats -a

# Hiển thị không liên tục
podman stats --no-stream mycontainer
```

### Container Top
```bash
# Xem processes trong container
podman top mycontainer

# Hiển thị với format
podman top mycontainer pid,ppid,cmd
```

---

## 3. Build Images

### Build từ Dockerfile
```bash
# Build image với Dockerfile
podman build -t myapp:latest .

# Build với file Dockerfile cụ thể
podman build -t myapp:latest -f Dockerfile.prod .

# Build không sử dụng cache
podman build --no-cache -t myapp:latest .

# Build với build arguments
podman build --build-arg NODE_VERSION=18 -t myapp:latest .

# Build với labels
podman build --label version=1.0 -t myapp:latest .

# Build và push lên registry
podman build -t myregistry.com/myapp:latest .
podman push myregistry.com/myapp:latest
```

### Commit Container to Image
```bash
# Tạo image từ container
podman commit mycontainer myapp:v1

# Tạo image với author và changes
podman commit --author="admin@example.com" mycontainer myapp:v1

# Tạo image với command mới
podman commit --change='CMD ["nginx", "-g", "daemon off;"]' mycontainer mynginx:latest
```

---

## 4. Volumes

### Volume Management
```bash
# Tạo volume
podman volume create mydata

# Liệt kê volumes
podman volume ls

# Kiểm tra volume
podman volume inspect mydata

# Xóa volume
podman volume rm mydata

# Xóa tất cả volumes không sử dụng
podman volume prune -f

# Mount volume vào container
podman run -v mydata:/data/data ubuntu:latest

# Tạo volume với driver cụ thể
podman volume create --driver local myvolume
```

---

## 5. Networks

### Network Management
```bash
# Liệt kê networks
podman network ls

# Tạo network
podman network create mynetwork

# Tạo network với subnet cụ thể
podman network create --subnet 172.20.0.0/16 mynetwork

# Kiểm tra network
podman network inspect mynetwork

# Xóa network
podman network rm mynetwork

# Xóa tất cả networks không sử dụng
podman network prune -f

# Chạy container với network tùy chỉnh
podman run --network=mynetwork --name web nginx:latest

# Disconnect container khỏi network
podman network disconnect mynetwork web

# Connect container vào network
podman network connect mynetwork web
```

---

## 6. Pods Management

### Pod Commands
```bash
# Tạo pod
podman pod create --name mypod

# Liệt kê pods
podman pod ls

# Chạy container trong pod
podman run --pod mypod -d nginx:latest

# Liệt kê containers trong pod
podman pod inspect mypod

# Xóa pod và tất cả containers trong pod
podman pod rm -f mypod
```

---

## 7. System Management

### System Info
```bash
# Xem thông tin hệ thống Podman
podman info

# Xem thông tin storage
podman system df

# Xem thông tin chi tiết về disk usage
podman system df -v
```

### Cleanup
```bash
# Dọn dẹp tất cả
podman system prune -a

# Dọn dẹp bao gồm volumes
podman system prune -a --volumes

# Dọn dẹp không có xác nhận
podman system prune -f
```

### Events
```bash
# Xem events
podman events

# Xem events với filter
podman events --filter container=mycontainer

# Xem events trong thời gian cụ thể
podman events --since 1h
```

---

## 8. Login/Registry

### Authentication
```bash
# Login vào registry
podman login docker.io

# Login vào registry cụ thể
podman login quay.io

# Logout
podman logout docker.io

# Login với credentials file
podman login --username $USER --password-stdin < credentials.txt
```

---

## 9. Import/Export

### Import/Export Images
```bash
# Export image thành tar
podman save -o myimage.tar myapp:latest

# Export nhiều images
podman save -o images.tar myapp:latest nginx:latest

# Export image với gzip
podman save --format tar -o myimage.tar.gz myapp:latest

# Import image từ tar
podman load -i myimage.tar

# Import với tên mới
podman load -i myimage.tar --name myapp:v2
```

### Import/Export Containers
```bash
# Export container filesystem thành tar
podman export mycontainer -o container.tar

# Import tar thành image
podman import container.tar myimported:v1
```

---

## 10. Common Options

### Global Options
```bash
# Chỉ định runtime
podman --runtime runc images

# Chỉ định cgroup driver
podman --cgroup-manager cgroupfs images

# Chỉ định log level
podman --log-level debug images

# Chỉ định configuration path
podman --config /path/to/config.json images
```

### Run Options Reference
| Option | Description |
|--------|-------------|
| `-d, --detach` | Chạy container ở background |
| `-i, --interactive` | STDIN mở |
| `-t, --tty` | Cấp phát pseudo-TTY |
| `--name` | Đặt tên container |
| `-p, --publish` | Port mapping |
| `-v, --volume` | Mount volume |
| `-e, --env` | Environment variable |
| `--env-file` | Đọc env từ file |
| `-m, --memory` | Memory limit |
| `--memory-swap` | Swap limit |
| `--cpus` | CPU limit |
| `--restart` | Restart policy |
| `--network` | Network mode |
| `--rm` | Xóa container khi exit |
| `--user` | User ID |
| `-w, --workdir` | Working directory |
| `--label` | Container labels |
| `--privileged` | Privileged mode |
| `--read-only` | Root filesystem read-only |

---

## 11. Docker Compatibility

### Migration từ Docker
```bash
# Docker compose compatibility
podman-compose up -d

# Sử dụng docker-compose.yml với podman
alias docker=podman

# Import docker images
docker save myimage:latest | podman load

# Export podman images cho docker
podman save myimage:latest | docker load
```

---

## 12. Useful Aliases

```bash
# Thêm vào ~/.bashrc hoặc ~/.zshrc
alias podmanps='podman ps --format "table {{.ID}}\t{{.Image}}\t{{.Status}}\t{{.Names}}"'
alias podmanimages='podman images --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}\t{{.Size}}"'
alias podmanlogs='podman logs --tail 50 -f'
alias podmanclean='podman system prune -af --volumes'
alias podmanstop='podman stop $(podman ps -q)'
```

---

## 13. Troubleshooting

```bash
# Kiểm tra Podman version
podman version

# Kiểm tra health của podman service
podman system connection list

# Xem logs của podman service (systemd)
journalctl -u podman -f

# Debug container
podman run --rm -it --entrypoint /bin/sh myimage

# Kiểm tra network connectivity
podman run --rm alpine ping -c 4 google.com

# Xem events chi tiết
podman events --filter type=container
```

---

## 14. Security Best Practices

```bash
# Chạy container với user không phải root
podman run --user 1000:1000 myimage

# Read-only container
podman run --read-only myimage

# Drop capabilities
podman run --cap-drop ALL myimage

# Thêm capabilities cần thiết
podman run --cap-add NET_ADMIN myimage

# Seccomp profile
podman run --security-opt seccomp=unconfined myimage
```

---

## 15. Examples Thực Tế

### Web Server
```bash
# Nginx web server
podman run -d \
  --name webserver \
  -p 80:80 \
  -v /var/www/html:/usr/share/nginx/html:ro \
  nginx:latest
```

### Database
```bash
# PostgreSQL
podman run -d \
  --name postgresdb \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_USER=appuser \
  -e POSTGRES_DB=mydb \
  -v pgdata:/var/lib/postgresql/data \
  postgres:15
```

### Node.js App
```bash
# Node.js application
podman run -d \
  --name nodeapp \
  -p 3000:3000 \
  -e NODE_ENV=production \
  -v $(pwd)/app:/app \
  -w /app \
  node:18-alpine \
  node index.js
```

### Development Environment
```bash
# Development với hot reload
podman run -it \
  --name dev-container \
  -p 8080:8080 \
  -v $(pwd):/workspace \
  -w /workspace \
  node:18-alpine \
  npm run dev
```

### Multi-container với Pod
```bash
# Tạo pod với web và db
podman pod create --name app-pod -p 8080:80

# Thêm container vào pod
podman run -d --pod app-pod --name web nginx:latest
podman run -d --pod app-pod --name db postgres:latest
```

---

## Tài liệu Tham Khảo

- [Podman Documentation](https://docs.podman.io/)
- [Podman GitHub](https://github.com/containers/podman)
- [Red Hat Podman](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/using-podman-to-manage-containers)
