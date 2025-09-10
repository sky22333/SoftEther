```
services:
  softether:
    image: ghcr.nju.edu.cn/ghcr.io/sky22333/softether
    container_name: l2tp
    restart: always
    privileged: true
    ports:
      - "500:500/udp"
      - "4500:4500/udp"
      - "1701:1701/udp"
    environment:
      - PSK=admin123
      - USERS=user1:password1;user2:password2;user3:password3
    cap_add:
      - NET_ADMIN
    volumes:
      - /lib/modules:/lib/modules
```

## 用户管理

### 添加用户
```bash
# 创建用户并设置密码
vpncmd localhost:443 /SERVER /HUB:DEFAULT /CMD UserCreate 用户名 /GROUP:none /REALNAME:none /NOTE:none
vpncmd localhost:443 /SERVER /HUB:DEFAULT /CMD UserPasswordSet 用户名 /PASSWORD:密码
```

### 删除用户
```bash
vpncmd localhost:443 /SERVER /HUB:DEFAULT /CMD UserDelete 用户名
```

### 查看用户
```bash
# 查看所有用户
vpncmd localhost:443 /SERVER /HUB:DEFAULT /CMD UserList

# 查看在线连接
vpncmd localhost:443 /SERVER /HUB:DEFAULT /CMD SessionList
```

> 💡 所有用户管理操作立即生效，无需重启容器

## Web 管理界面

### 启用 Web 管理
```bash
# 启用 HTTPS 管理界面
vpncmd localhost:443 /SERVER /CMD HttpsEnable yes /PORT:5555
```

### 修改配置文件
在 `docker-compose.yml` 中添加端口映射：
```yaml
ports:
  - "500:500/udp"
  - "4500:4500/udp"
  - "1701:1701/udp"
  - "5555:5555/tcp"  # Web 管理端口
```

### 访问界面
- **地址**: `https://localhost:5555/`
- **服务器**: `localhost:443`
- **HUB**: `DEFAULT`
- **密码**: 首次访问时设置

> ⚠️ 浏览器会显示证书警告，点击"继续访问"即可