```yaml
services:
  softether:
    image: ghcr.io/sky22333/softether
    container_name: l2tp
    restart: always
    network_mode: host
    environment:
      - PSK=admin123
      - USERS=user1:password1;user2:password2;user3:password3

      # 管理密码
      - HPW=admin789
      - SPW=admin789
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
