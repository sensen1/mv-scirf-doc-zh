# scikit-rf 微波工程教学环境部署指南

## 概述

本指南介绍如何部署基于 JupyterHub 的微波工程教学环境，集成 scikit-rf 和 pycharge 两个开源项目。

## 服务器要求

- **CPU**: 64 核心 (EPYC 2.8GHz)
- **内存**: 256GB
- **操作系统**: Ubuntu Server 24.x
- **磁盘**: 至少 100GB 可用空间
- **网络**: 可访问互联网

## 交付文件清单

```
output/
├── Dockerfile                      # JupyterHub + scikit-rf + pycharge 镜像
├── docker-compose.yml              # Docker Compose 部署配置
├── docker-entrypoint.sh            # 容器入口脚本
├── jupyterhub_config.py            # JupyterHub 配置文件
├── DEPLOYMENT_GUIDE.md             # 本部署指南
├── skrf-doc-zh/                    # 中文文档
│   ├── source/                     # 文档源文件
│   │   ├── conf.py                 # Sphinx 配置
│   │   ├── index.rst               # 文档首页
│   │   ├── tutorials/              # 教程 notebooks
│   │   ├── examples/               # 示例
│   │   ├── api/                    # API 文档
│   │   └── ...
│   └── Makefile                    # 文档构建
└── nginx/                          # Nginx 配置 (可选)
    └── nginx.conf
```

---

## 部署方案一：Docker Compose 部署（推荐）

### 1. 安装 Docker 和 Docker Compose

```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装 Docker
sudo apt install -y docker.io docker-compose-plugin

# 启动 Docker 服务
sudo systemctl enable docker
sudo systemctl start docker

# 将当前用户添加到 docker 组
sudo usermod -aG docker $USER
# 重新登录以应用组更改
```

### 2. 构建并启动服务

```bash
# 克隆或复制项目文件到服务器
cd /opt
mkdir -p microwave-lab
cd microwave-lab

# 复制所有交付文件到该目录
# ...

# 构建镜像
sudo docker build -t microwave-lab:jupyterhub-latest .

# 启动服务
sudo docker-compose up -d
```

### 3. 访问 JupyterHub

- 打开浏览器访问: `http://your-server-ip:8000`
- 默认管理员账号: `admin`
- 默认管理员密码: `changeme` (请在生产环境中修改)

### 4. 常用命令

```bash
# 查看服务状态
sudo docker-compose ps

# 查看日志
sudo docker-compose logs -f jupyterhub

# 停止服务
sudo docker-compose down

# 重启服务
sudo docker-compose restart

# 更新配置后重新构建
sudo docker-compose up -d --build
```

---

## 部署方案二：使用 Docker Swarm（生产环境）

### 1. 初始化 Swarm

```bash
sudo docker swarm init
```

### 2. 部署 Stack

```bash
sudo docker stack deploy -c docker-compose.yml microwave-lab
```

### 3. 查看服务

```bash
sudo docker stack ps microwave-lab
sudo docker service logs microwave-lab_jupyterhub
```

---

## 部署方案三：Kubernetes 部署（大规模）

### 1. 创建 Namespace

```bash
kubectl create namespace microwave-lab
```

### 2. 创建 ConfigMap

```bash
kubectl create configmap jupyterhub-config --from-file=jupyterhub_config.py -n microwave-lab
```

### 3. 创建 Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jupyterhub
  namespace: microwave-lab
spec:
  replicas: 1
  selector:
    matchLabels:
      app: jupyterhub
  template:
    metadata:
      labels:
        app: jupyterhub
    spec:
      containers:
      - name: jupyterhub
        image: microwave-lab:jupyterhub-latest
        ports:
        - containerPort: 8000
        env:
        - name: JUPYTERHUB_ADMIN_USER
          value: "admin"
        volumeMounts:
        - name: config
          mountPath: /etc/jupyterhub
        - name: data
          mountPath: /home
      volumes:
      - name: config
        configMap:
          name: jupyterhub-config
      - name: data
        persistentVolumeClaim:
          claimName: jupyterhub-data
```

### 4. 创建 Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: jupyterhub
  namespace: microwave-lab
spec:
  selector:
    app: jupyterhub
  ports:
  - port: 8000
    targetPort: 8000
  type: LoadBalancer
```

---

## 中文文档部署

### 方案一：Read the Docs 部署（推荐）

1. 在 [Read the Docs](https://readthedocs.org/) 创建账号
2. 导入 GitHub 仓库
3. 在项目中设置:
   - 语言: 简体中文
   - Sphinx 配置: `skrf-doc-zh/source/conf.py`
   - Python 版本: 3.10+
   - 依赖: 安装 `sphinx`, `sphinx-rtd-theme`, `nbsphinx`

### 方案二：自建服务器部署

```bash
# 进入中文文档目录
cd skrf-doc-zh

# 安装依赖
pip install sphinx sphinx-rtd-theme nbsphinx

# 构建 HTML 文档
make html

# 文档将生成在 build/html 目录
# 可以使用 Nginx 或 Apache 提供服务
```

### 方案三：GitHub Pages 部署

```bash
# 安装 ghp-import
pip install ghp-import

# 构建并部署
cd skrf-doc-zh
make html
ghp-import -n -p -f build/html/
```

### 方案四：Netlify/Vercel 部署

1. 将代码推送到 GitHub
2. 在 Netlify/Vercel 中连接仓库
3. 构建设置:
   - Build command: `cd skrf-doc-zh && make html`
   - Publish directory: `skrf-doc-zh/build/html`

---

## SSL/HTTPS 配置

### 使用 Let's Encrypt

```bash
# 安装 certbot
sudo apt install -y certbot

# 获取证书
sudo certbot certonly --standalone -d your-domain.com

# 修改 docker-compose.yml 挂载证书
# 然后重启服务
```

### 使用 Nginx 反向代理

```bash
# 启动 nginx 服务
docker-compose --profile production up -d nginx
```

---

## 用户管理

### 创建新用户

```bash
# 进入 JupyterHub 容器
sudo docker exec -it microwave-jupyterhub bash

# 创建用户
useradd -m -s /bin/bash username
passwd username
```

### 配置身份验证

编辑 `jupyterhub_config.py`:

```python
# 使用 LDAP 认证
c.JupyterHub.authenticator_class = 'ldapauthenticator.LDAPAuthenticator'
c.LDAPAuthenticator.server_address = 'ldap.example.com'
c.LDAPAuthenticator.bind_dn_template = 'uid={username},ou=people,dc=example,dc=com'

# 或使用 OAuth
c.JupyterHub.authenticator_class = 'oauthenticator.GitHubOAuthenticator'
c.GitHubOAuthenticator.client_id = 'your-client-id'
c.GitHubOAuthenticator.client_secret = 'your-client-secret'
```

---

## 资源管理

### 设置用户资源限制

编辑 `docker-compose.yml`:

```yaml
environment:
  - SPAWN_CPU_LIMIT=4.0      # 每用户 CPU 限制
  - SPAWN_MEM_LIMIT=16G      # 每用户内存限制
```

### 自动清理空闲服务器

```yaml
environment:
  - CULL_IDLE_TIMEOUT=3600   # 1小时后清理空闲服务器
  - CULL_MAX_AGE=86400       # 24小时后强制清理
```

---

## 备份与恢复

### 备份用户数据

```bash
# 备份卷
sudo docker run --rm -v jupyterhub-data:/data -v $(pwd):/backup alpine tar czf /backup/jupyterhub-backup.tar.gz -C /data .
```

### 恢复数据

```bash
# 恢复卷
sudo docker run --rm -v jupyterhub-data:/data -v $(pwd):/backup alpine tar xzf /backup/jupyterhub-backup.tar.gz -C /data
```

---

## 故障排除

### 查看日志

```bash
sudo docker-compose logs -f jupyterhub
```

### 常见问题

1. **端口被占用**
   ```bash
   sudo netstat -tlnp | grep 8000
   sudo kill -9 <PID>
   ```

2. **权限问题**
   ```bash
   sudo chown -R 1000:100 /home/shared
   ```

3. **磁盘空间不足**
   ```bash
   sudo docker system prune -a
   ```

---

## 安全建议

1. **修改默认密码**: 立即修改 admin 默认密码
2. **启用 HTTPS**: 使用 SSL 证书保护通信
3. **防火墙配置**: 只开放必要的端口 (80, 443, 8000)
4. **定期更新**: 定期更新 Docker 镜像和系统补丁
5. **访问控制**: 配置适当的用户认证机制

---

## 联系与支持

- scikit-rf 官方文档: https://scikit-rf.readthedocs.io/
- scikit-rf GitHub: https://github.com/scikit-rf/scikit-rf
- pycharge GitHub: https://github.com/MatthewFilipovich/pycharge
- JupyterHub 文档: https://jupyterhub.readthedocs.io/
