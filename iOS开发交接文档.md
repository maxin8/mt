# MT 字幕翻译官 · 开发维护交接文档

## 一、产品概述

**产品名称**：MT 字幕翻译官  
**当前版本**：Windows 桌面版 v1.0.2（已上线）  
**目标**：开发 iOS 版本，复用现有服务端激活体系，适配移动端使用场景  
**开发者邮箱**：qqmaxing@gmail.com  
**GitHub 仓库**：https://github.com/maxin8/mt

---

## 二、版本更新记录

### v1.0.2（2026-06-08）
- 新增**标题翻译**页面：手动输入大段文字，一键翻译为多种语言，所有语言合并保存到同一个 txt 文件，顶部附原文对照，语言间星号分隔，Alt+Enter 快捷键触发，点击文件名直接打开
- **即时互译**（原"文字互译"）新增快捷键：Ctrl+C 智能复制全文（有选中文字时优先复制选中）、Alt+D 一键清空输入和结果
- 修复即时互译回车换行问题（之前回车会误触发翻译）
- 复制按钮状态优化：复制后显示"已复制"，输入新内容或清空后自动恢复
- 落地页 moxidao.com/mt 同步更新，下载链接改为 GitHub Releases 页面并加下载说明

### v1.0.1（2026-06-06）
- 初始发布，SRT 字幕翻译 + 即时互译 + 激活码系统

---

## 三、Windows 版核心功能（参考实现）

| 功能 | 说明 |
|------|------|
| SRT 字幕翻译 | 导入 .srt 文件 → 选目标语言 → 调 AI API 逐条翻译 → 导出新 .srt |
| 标题翻译 | 手动输入大段文字 → 选多种目标语言 → 翻译结果+原文合并保存为单个 txt |
| 即时互译 | 输入框实时翻译，Ctrl+C 智能复制，Alt+D 清空，支持语言自由切换 |
| 多语言支持 | 20 种语言（简体中文、英语、日语、韩语、法语等） |
| 多平台 API | DeepSeek、阿里云通义、OpenAI、Claude、Google Gemini（用户自填 API Key） |
| 激活码授权 | 一机一码，1 年有效期，支持续费叠加 |
| 全局快捷键 | Alt+T 呼出主窗口 |

---

## 四、服务端基础设施（可直接复用）

### flamebush.com 服务器
- **IP**：107.174.11.28
- **域名**：flamebush.com / www.flamebush.com
- **系统**：Ubuntu 24.04
- **Web 路径**：`/var/www/flamebush/`
- **SSH 用户**：root
- **SSH 密码**：Z70gKld4g4E1WtmCL7
- **SSH 命令**：`ssh -o PubkeyAuthentication=no root@107.174.11.28`

### moxidao.com 网站（FTP）
- **FTP 地址**：ftp.vhost012.51web.cn
- **用户名**：web80063
- **密码**：kuSTkuuT
- **Web 根目录**：/web80063/web/
- **MT 落地页**：moxidao.com/mt/index.html

### 数据库（MySQL，flamebush.com 上）
- **库名**：carddb
- **用户**：carduser
- **密码**：Card@2025mT!
- **关键表**：`activation_codes`（status / machine_id / expires_at）

### 爱发电支付平台
- **用户 ID**：63b5098e60dd11f19fc752540025c377
- **Token**：6XnjFMEeTCrqtg3uy9pbUJhfQRaWx7mS
- **Plan ID**：73a32eac60e211f18e745254001e7c00
- **Webhook Token**：MTSubPro2025Hook
- **后台管理密码**：admin888
- **Webhook 地址**：http://www.flamebush.com/webhook.php?token=MTSubPro2025Hook

### DeepSeek API（开发者测试用）
- **API Key**：sk-c13e237ba40841d0a3c6388a0b5bd6aa

### PHP API 端点

#### 激活验证（客户端调用）
```
POST http://www.flamebush.com/validate.php
Content-Type: application/json

{
  "code": "XXXXX-XXXXX-XXXXX",
  "machine_id": "设备唯一标识符"
}
```

**返回示例（成功）**：
```json
{ "status": "ok", "expires_at": "2027-06-06 05:32:51", "message": "激活成功" }
```

**返回示例（失败）**：
```json
{ "status": "error", "message": "激活码不存在" }
```

---

## 五、GitHub 发版流程（无需安装 git，用 API 操作）

> 本机没有安装 git，所有 GitHub 操作通过 Python + GitHub REST API 完成。

### 基本信息
- **仓库**：`maxin8/mt`
- **GitHub Token**：`ghp_xxxxxxxx（见本地交接文档）`（有 repo 权限）
- **Token 放置**：只存在本文档，不要硬编码进软件源码

### 5.1 发布新版本（创建 Release + 上传 exe）

```python
import requests, os

TOKEN   = "ghp_xxxxxxxx（见本地交接文档）"
REPO    = "maxin8/mt"
TAG     = "v1.0.3"   # ← 改成新版本号
EXE     = r"C:\Users\梁AI\Desktop\srt_translator\dist\MT字幕翻译官.exe"
HEADERS = {"Authorization": f"Bearer {TOKEN}", "Accept": "application/vnd.github+json"}

# 1. 创建 Release
body = {
    "tag_name": TAG,
    "name": TAG,
    "body": "## 更新内容\n- ...",   # ← 填写更新日志
    "draft": False,
    "prerelease": False,
}
r = requests.post(f"https://api.github.com/repos/{REPO}/releases", headers=HEADERS, json=body)
release_id = r.json()["id"]
upload_base = r.json()["upload_url"].split("{")[0]
print("Release created:", release_id)

# 2. 上传 exe
with open(EXE, "rb") as f:
    data = f.read()
upload_headers = {**HEADERS, "Content-Type": "application/octet-stream"}
r2 = requests.post(f"{upload_base}?name=mt.exe", headers=upload_headers, data=data)
print("Download URL:", r2.json().get("browser_download_url"))
```

### 5.2 更新 README.md

```python
import requests, base64

TOKEN   = "ghp_xxxxxxxx（见本地交接文档）"
REPO    = "maxin8/mt"
HEADERS = {"Authorization": f"Bearer {TOKEN}", "Accept": "application/vnd.github+json"}

# 获取当前文件 SHA（更新文件必须带 SHA）
r = requests.get(f"https://api.github.com/repos/{REPO}/contents/README.md", headers=HEADERS)
sha = r.json()["sha"]

# 读取本地 README
with open(r"C:\Users\梁AI\Desktop\srt_translator\README.md", "r", encoding="utf-8") as f:
    content = f.read()

# 推送
body = {
    "message": "docs: update README",
    "content": base64.b64encode(content.encode("utf-8")).decode(),
    "sha": sha,
}
r2 = requests.put(f"https://api.github.com/repos/{REPO}/contents/README.md", headers=HEADERS, json=body)
print("Status:", r2.status_code)
```

### 5.3 完整发版步骤

每次发新版本，按顺序执行：

1. 更新 `translator.py` 里的 `VERSION = "x.x.x"`
2. 更新 `README.md` 的更新日志
3. 用 PyInstaller 重新打包：`python -m PyInstaller MT字幕翻译官.spec --clean`
4. 运行 5.1 的脚本创建 Release 并上传 exe
5. 运行 5.2 的脚本推送 README
6. 如需更新落地页，用 FTP 上传 `moxidao.com/mt/index.html`

---

## 六、FTP 文件更新方法（moxidao.com）

```python
from ftplib import FTP

ftp = FTP()
ftp.encoding = 'gbk'          # 51web 服务器用 GBK 编码
ftp.connect('ftp.vhost012.51web.cn')
ftp.login('web80063', 'kuSTkuuT')
ftp.set_pasv(True)
ftp.cwd('web/mt')

# 上传文件
with open(r'C:\Users\梁AI\Desktop\mt_index.html', 'rb') as f:
    ftp.storbinary('STOR index.html', f)

ftp.quit()
print('上传成功')
```

> 注意：FTP 登录用 `ftp.encoding = 'gbk'` 否则会报 UnicodeDecodeError。路径从登录后的 `/web80063/` 开始，所以 `cwd('web/mt')` 对应网站根目录的 `/mt/`。

---

## 七、Mac 上使用 Claude Code（含 VPN 登录指南）

> **重要**：Claude Code 需要访问 Anthropic 的服务器，国内网络无法直接连接，**必须先开启 VPN，再启动 Claude Code**，否则会提示连接失败或登录失败。

### 7.1 第一次登录步骤

**第一步：开启 VPN**
- 打开你使用的 VPN 软件（如 Clash、Surge 等），确认已连接，能正常访问 Google
- **VPN 必须在开启 Claude Code 之前打开**，不能先开软件再开 VPN

**第二步：打开终端**
- 方法一：按 `Command + 空格`，输入 `Terminal`，回车
- 方法二：打开 `访达（Finder）` → `应用程序` → `实用工具` → `终端`
- 终端打开后会看到一个黑色或白色的命令行窗口，光标在闪烁

**第三步：启动 Claude Code**
- 在终端里输入以下命令，回车：
  ```
  claude
  ```
- 如果提示 `command not found`，说明还没安装，先运行：
  ```
  npm install -g @anthropic-ai/claude-code
  ```
  如果 npm 也没有，先安装 Node.js：去 https://nodejs.org 下载 LTS 版本安装

**第四步：登录授权**
- 首次运行 `claude` 会自动打开浏览器，跳转到 Claude 登录页
- 用你的 claude.ai 账号登录（邮箱：maxsongbo@gmail.com）
- 登录成功后浏览器会提示"已授权"，回到终端即可开始使用

### 7.2 日常使用

每次使用 Claude Code 前：
1. 先开 VPN ✓
2. 打开终端
3. 输入 `claude` 回车，直接进入对话

如果之前登录过，不需要重复授权，直接对话即可。

### 7.3 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| `Error: connect ECONNREFUSED` | VPN 未开启 | 先开 VPN 再运行 |
| `command not found: claude` | 未安装 | `npm install -g @anthropic-ai/claude-code` |
| `command not found: npm` | 未安装 Node.js | 去 nodejs.org 下载安装 |
| 浏览器打开但登录页加载失败 | VPN 未正常代理浏览器 | 检查 VPN 是否开了全局模式 |
| 登录后终端没有反应 | 正常，稍等几秒 | 等待终端出现提示符即可 |

### 7.4 让 Claude Code 帮你做事

进入 Claude Code 后，直接用中文描述任务即可，例如：

- `帮我更新 translator.py 里的版本号为 1.0.3，然后重新打包`
- `把 README 的更新日志加上最新版本内容`
- `用 GitHub API 把新的 exe 上传到 releases`

Claude Code 可以直接读写你电脑上的文件、运行 Python 脚本、调用 API，不需要你手动操作。

---

## 八、iOS 版设备唯一标识（替代 Windows MachineGuid）

Windows 版用 `MachineGuid` 哈希作为设备 ID。iOS 无此机制，推荐方案：

```swift
import UIKit

func getDeviceID() -> String {
    let key = "MT_DeviceID"
    if let saved = UserDefaults.standard.string(forKey: key) {
        return saved
    }
    let id = UIDevice.current.identifierForVendor?.uuidString ?? UUID().uuidString
    let short = String(id.replacingOccurrences(of: "-", with: "").prefix(16)).uppercased()
    UserDefaults.standard.set(short, forKey: key)
    return short
}
```

**建议**：用 Keychain 存储，卸载重装后 machine_id 不变，用户无需重新激活。

---

## 九、iOS 版建议技术栈

| 层 | 推荐 |
|----|------|
| UI 框架 | SwiftUI（iOS 16+） |
| 网络请求 | URLSession 或 Alamofire |
| 本地存储 | UserDefaults + Keychain |
| SRT 解析 | 自写 Parser（格式简单，见下方） |
| AI 翻译 API | 与 Windows 版相同的 HTTP 接口 |
| 构建工具 | Xcode 15+，Swift 5.9+ |
| 最低系统 | iOS 16.0 |

---

## 十、SRT 格式说明（解析参考）

```
1
00:00:01,000 --> 00:00:03,500
Hello world

2
00:00:04,000 --> 00:00:06,000
This is subtitle line two
```

每条字幕由三部分组成：序号 / 时间码 / 文字内容，空行分隔。  
Windows 版解析逻辑在 `translator.py` 的 `parse_srt()` 函数中，可直接参考移植为 Swift。

---

## 十一、翻译 API 调用方式

以 DeepSeek 为例（其他平台格式类似）：

```
POST https://api.deepseek.com/chat/completions
Authorization: Bearer {用户填写的 API Key}
Content-Type: application/json

{
  "model": "deepseek-chat",
  "messages": [
    { "role": "user", "content": "Translate to English. Return ONLY translation:\n\n{原文}" }
  ],
  "max_tokens": 4096
}
```

iOS 版建议每次翻译批量打包多条字幕（10～20 条）发送，减少请求次数。

---

## 十二、iOS 版 App 结构建议

```
MT字幕翻译官/
├── App/
│   └── MTApp.swift
├── Views/
│   ├── MainTabView.swift
│   ├── SRTTranslateView.swift      // SRT 字幕翻译
│   ├── TitleTranslateView.swift    // 标题翻译（多语言合并保存）
│   ├── LiveTranslateView.swift     // 即时互译
│   ├── SettingsView.swift          // API 设置
│   └── ActivateView.swift          // 激活
├── Models/
│   ├── SRTParser.swift
│   ├── TranslationService.swift
│   └── LicenseManager.swift
├── Utils/
│   ├── KeychainHelper.swift
│   └── DeviceID.swift
└── Resources/
    └── Languages.swift             // 20 种语言列表
```

---

## 十三、激活逻辑（iOS 版对应实现）

```swift
struct LicenseManager {
    static func activate(code: String) async -> Result<Date, Error> {
        let deviceID = DeviceID.get()
        let body = ["code": code, "machine_id": deviceID]
        // POST http://www.flamebush.com/validate.php
        // 成功则解析 expires_at 存入 Keychain
    }

    static func isActive() -> Bool {
        guard let expiry = getExpiry() else { return false }
        return expiry > Date()
    }

    static func isExpiringSoon() -> Bool {
        guard let expiry = getExpiry() else { return false }
        return expiry.timeIntervalSinceNow < 7 * 86400
    }
}
```

---

## 十四、iOS vs Windows 关键差异

| 项目 | Windows 版 | iOS 版注意事项 |
|------|-----------|--------------|
| 文件选取 | filedialog | UIDocumentPickerViewController |
| 文件保存 | 任意路径 | 只能存到沙盒或通过 Share Sheet 导出 |
| 设备 ID | MachineGuid (注册表) | Keychain 存储 UUID |
| 后台运行 | 无限制 | 翻译任务需在前台或用 Background Task |
| 快捷键 | Alt+T / Ctrl+C / Alt+D | 不适用，可改为手势或工具栏按钮 |
| 安装包 | .exe 单文件 | .ipa，须 Apple Developer 账号（$99/年）分发 |

---

## 十五、上线前必做事项

1. **Apple Developer 账号**：$99/年，注册地址 developer.apple.com
2. **App Store 审核**：激活码付费模式需确认不违反 IAP 政策
3. **HTTPS**：validate.php 目前是 HTTP，App Store 强制要求 HTTPS  
   → 需给 flamebush.com 配置 SSL 证书（Let's Encrypt 免费）  
   → 可告诉 Claude Code：「帮我给 flamebush.com 配置 Let's Encrypt，443 被 Xray 占用，用 acme.sh + DNS 验证方式」
4. **隐私政策页面**：App Store 要求必须有隐私政策 URL

---

## 十六、本地 Windows 版源码位置

| 文件 | 路径 |
|------|------|
| 主程序 | `C:\Users\梁AI\Desktop\srt_translator\translator.py` |
| 配置文件 | `C:\Users\梁AI\Desktop\srt_translator\config.json` |
| 打包输出 | `C:\Users\梁AI\Desktop\srt_translator\dist\MT字幕翻译官.exe` |
| 打包配置 | `C:\Users\梁AI\Desktop\srt_translator\MT字幕翻译官.spec` |
| 服务端脚本 | `C:\Users\梁AI\Desktop\cardserver\` |
| 图标源文件 | `C:\Users\梁AI\Desktop\srt_translator\app.ico` |
| 落地页（本地） | `C:\Users\梁AI\Desktop\mt_index.html` |

---

*文档最后更新：2026-06-08*
