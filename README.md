# 🕸️ evesiteclone

**Eve SiteClone** 是一个网页抓取与镜像工具，可以将指定网站完整抓取（包括 HTML 页面、CSS、JS、图片等静态资源），并保存为可离线访问的本地副本。同时支持 **增量更新** 和 **FTP 自动上传**，可用于：  

- 网站归档  
- 数据采集  
- 网站搬家/迁移  
- 静态镜像部署  

---

## 🚀 功能特性

- 📂 **多页面递归抓取**：支持递归抓取整个站点。  
- 🖼️ **静态资源下载**：CSS、JS、图片等全部下载，保证离线可用。  
- 🔄 **增量更新**：仅在页面内容变化时更新文件。  
- 📊 **抓取进度提示**：显示已抓取、剩余页面数。  
- 📝 **错误重试机制**：支持页面和资源的重试。  
- 🌐 **代理支持**：可选择是否通过 HTTP 代理访问。  
- 📤 **FTP 上传**：抓取完成后自动上传到远程服务器。  

---

## ⚙️ 环境变量配置

容器支持通过环境变量灵活配置抓取和上传参数。  

| 环境变量 | 默认值 | 说明 |
|----------|--------|------|
| `TARGET_URLS` | *(必填)* | 目标网站列表，多个 URL 用逗号分隔 |
| `OUTPUT_DIR` | `/output` | 抓取结果保存目录（挂载到宿主机） |
| `CHECK_INTERVAL` | `3600` | 抓取循环间隔（秒） |
| `HTTP_PROXY` | *(空)* | HTTP 代理地址，例如 `http://192.168.6.2:43002` |
| `USE_PROXY` | `true` | 是否启用代理 (`true`/`false`) |
| `RETRY_COUNT` | `3` | 失败重试次数 |
| `TIMEOUT` | `20` | 抓取超时时间（秒） |
| `FTP_ENABLED` | `false` | 是否启用 FTP 上传 (`true`/`false`) |
| `FTP_HOST` | *(空)* | FTP 服务器地址 |
| `FTP_PORT` | `21` | FTP 端口 |
| `FTP_USER` | *(空)* | FTP 用户名 |
| `FTP_PASS` | *(空)* | FTP 密码 |
| `FTP_TARGET_DIR` | *(空)* | FTP 上传目标目录 |

---

## 🐳 使用方法

### 1. 运行容器（最小示例）

```bash
docker run -d \
  --name eve-siteclone \
  -e TARGET_URLS="https://example.com,https://another.com/page" \
  -v /path/on/host/output:/output \
  your-dockerhub-username/eve-siteclone:latest
2. 使用代理
docker run -d \
  --name eve-siteclone \
  -e TARGET_URLS="https://example.com" \
  -e USE_PROXY=true \
  -e HTTP_PROXY="http://192.168.6.2:43002" \
  -v /path/on/host/output:/output \
  your-dockerhub-username/eve-siteclone:latest

3. 启用 FTP 上传
docker run -d \
  --name eve-siteclone \
  -e TARGET_URLS="https://example.com" \
  -e FTP_ENABLED=true \
  -e FTP_HOST="120.70.154.25" \
  -e FTP_USER="username" \
  -e FTP_PASS="password" \
  -v /path/on/host/output:/output \
  your-dockerhub-username/eve-siteclone:latest

📊 输出目录结构

抓取后的数据将保存到挂载目录下，例如：

/output/
  ├── example.com/
  │   ├── index.html
  │   ├── style.css
  │   ├── script.js
  │   └── images/
  └── another.com/
      └── index.html

🔧 Docker Compose 示例
version: "3"
services:
  eve-siteclone:
    image: your-dockerhub-username/eve-siteclone:latest
    container_name: eve-siteclone
    restart: always
    environment:
      - TARGET_URLS=https://example.com
      - CHECK_INTERVAL=3600
      - FTP_ENABLED=false
    volumes:
      - ./output:/output
