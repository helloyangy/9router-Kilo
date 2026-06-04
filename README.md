# 9Router

Kilo 提供免费的大模型，但是没 API，就需要反代把它提取成 API。

![9Router 截图](./_resources/65ccbda363a3e3c6359bd1e42da5ad0e.png)

项目地址：https://github.com/decolua/9router

## 推荐配置

推荐服务器部署，不要选择国内地区，选择 Linux 版本 Docker 上手快。

腾讯云新加坡、硅谷、东京地区价格是 199 元一年，2 核 4G 30M 带宽，60GB SSD 盘，1.5T 月流量，推荐首尔线路，系统选 Ubuntu 24。（可以同价续费）

购买地址：https://curl.qcloud.com/oyWDLkRJ

![服务器配置截图](./_resources/72d493eab65af6588b3ed9cb7585b177.png)

## 教程

### 1. Ubuntu 24 系统安装 Docker

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```

![Docker 安装成功截图](./_resources/a574344baf10d455c32ec479ede1d270.png)

### 2. Docker 部署项目

推荐设置一个强密码。

```bash
docker run -d \
  --name 9router \
  --restart=unless-stopped \
  -p 20128:20128 \
  -v "$HOME/.9router:/app/data" \
  -e DATA_DIR=/app/data \
  -e JWT_SECRET='换成超长随机字符串' \
  -e INITIAL_PASSWORD='换成强密码' \
  -e HOSTNAME=0.0.0.0 \
  -e REQUIRE_API_KEY=true \
  decolua/9router:latest
```

示例命令可以直接复制，推荐修改密码：

```bash
docker run -d \
  --name 9router \
  --restart=unless-stopped \
  -p 20128:20128 \
  -v "$HOME/.9router:/app/data" \
  -e DATA_DIR=/app/data \
  -e JWT_SECRET='9router-prod-jwt-2026-05-30-x8FvK2mQp7Nz4LdRc1TwUa6Hy9Sb3JeM' \
  -e INITIAL_PASSWORD='123456' \
  -e HOSTNAME=0.0.0.0 \
  -e REQUIRE_API_KEY=true \
  decolua/9router:latest
```

![部署成功截图](./_resources/06e37bbb8c3ce09e8f33e6230a2a7f34.png)

### 3. 浏览器访问，防火墙放通 20128 端口

```
http://你的服务器IP:20128
```

![面板访问截图](./_resources/319688db3326ac1e384d36372961264c.png)

### 4. Providers 选择 Kilo Code

我这里选择的模型是 `kc/nvidia/nemotron-3-ultra-550b-a55b:free`。

![Provider 配置截图](./_resources/ca5c14a88d0ca007f8d8911a799a53ee.png)

### 5. 调用测试成功

接口：`http://你的服务器IP:20128/v1`

模型：`kc/nvidia/nemotron-3-ultra-550b-a55b:free`

![调用测试成功截图](./_resources/975379005de3777a55f2ef7fbfee91d9.png)
