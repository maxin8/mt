# MT 字幕翻译官 · MT Subtitle Pro

专为视频创作者打造的 SRT 字幕 & 文字翻译工具，支持 20 种语言，AI 驱动，一键批量翻译。

*A subtitle & text translation tool built for video creators — 20 languages, AI-powered, one-click batch translation.*

[⬇ 下载最新版本 / Download Latest](https://github.com/maxin8/mt/releases/latest) | 邮箱 / Email：qqmaxing@gmail.com

---

## 主要功能 / Key Features

### SRT 字幕翻译 / SRT Subtitle Translation
导入 SRT 文件，勾选目标语言（可多选），点击开始翻译，自动生成对应语言字幕文件并保存到指定目录。支持 20 种语言批量同时翻译。

*Import an SRT file, select target languages (multi-select), click Translate — subtitle files are automatically generated and saved. Supports batch translation into 20 languages simultaneously.*

### 即时互译 / Live Translation
粘贴或输入任意文字，边输入边翻译，实时显示结果。支持自由切换源语言和目标语言，⇄ 互换并重新翻译。Ctrl+C 智能复制，Alt+D 一键清空。

*Paste or type any text and see the translation in real time. Swap source/target languages with ⇄. Ctrl+C smart copy, Alt+D clear.*

### 标题翻译 / Title Translation
输入视频标题或大段文字，一键翻译为多种语言，所有语言合并保存为一个 TXT 文件，顶部附原文对照。Alt+Enter 快捷键触发。

*Translate video titles or long text into multiple languages at once. All results are saved into a single TXT file with the original text at the top. Shortcut: Alt+Enter.*

### 封面生成 / Thumbnail Generator
根据标题翻译结果，自动渲染横屏（1280×720）或竖屏（1080×1920）封面图。可调标题位置、字号、文字颜色、背景色带和描边颜色；支持 AI 优化文案后再渲染。

*Auto-render landscape (1280×720) or portrait (1080×1920) thumbnails from translation results. Adjust text position, size, color, background strip, and stroke. AI text polish supported.*

### AI 智能生成标题 / AI Title Generator
SRT 翻译完成后自动生成凝练版标题、标题党标题和内容摘要，一键填入「标题翻译」框。支持标题风格（简中 / 适中 / 详细）和摘要长度设置。

*After SRT translation, automatically generates a concise title, a click-bait title, and a content summary — pre-filled into the Title Translation box. Adjustable style and summary length.*

### 多平台 API 支持 / Multi-Platform API Support
支持 DeepSeek、阿里云通义千问、OpenAI GPT、Claude（Anthropic）、Google Gemini 五大翻译引擎。点击任意平台行即切换，高级设置可为每个平台单独配置中转代理地址。

*Supports DeepSeek, Alibaba Qwen, OpenAI GPT, Claude (Anthropic), and Google Gemini. Click any platform row to switch instantly. Advanced settings allow per-platform proxy/relay URL.*

### 界面多语言 / Multilingual UI
软件界面支持 **简体中文 / 繁体中文 / English / Español / Français** 五种语言切换，点击右上角按钮即时切换，所有提示和报错信息随界面语言同步变化。

*The app interface supports **Simplified Chinese / Traditional Chinese / English / Español / Français**. Switch instantly via the top-right buttons — all messages and errors follow the selected language.*

### 激活码授权 / License System
- 免费版 / Free：简体中文、繁体中文、英语 / Simplified Chinese, Traditional Chinese, English
- 满血版 / Full：全部 20 种语言 / All 20 languages
- 一机一码，重装免费重激活 / One license per device, free re-activation after reinstall
- 有效期 1 年，续费从到期日顺延 / 1-year validity, renewal extends from expiry date

---

## 下载安装 / Installation

前往 [Releases 页面](https://github.com/maxin8/mt/releases/latest) 下载 `mt.exe`，双击即可运行，**无需安装，无需配置环境**。

*Download `mt.exe` from the [Releases page](https://github.com/maxin8/mt/releases/latest) and double-click to run. **No installation or environment setup required.***

---

## ⚠️ Windows 11 无法打开 / 被拦截怎么办？
## ⚠️ Can't open on Windows 11? Here's how to fix it.

Windows 11 会对未经微软代码签名的独立开发者软件进行拦截，属于正常的安全机制，软件本身不含任何恶意代码。

*Windows 11 blocks unsigned software from independent developers by default — this is normal behavior. The app contains no malicious code.*

### 情况一 / Case 1：弹出「Windows 已保护你的电脑」 / "Windows protected your PC" dialog

1. 点击 **"更多信息"** / Click **"More info"**
2. 点击 **"仍要运行"** / Click **"Run anyway"**

### 情况二 / Case 2：文件被锁定 / File is "blocked"

1. 右键 `mt.exe` → **属性 / Properties**
2. 勾选底部 **"解除锁定 / Unblock"** 复选框
3. 点击 **确定 / OK**，再次双击运行

### 情况三 / Case 3：杀毒软件删除或隔离 / Antivirus quarantined the file

- 打开杀毒软件 → 隔离区 → 恢复文件
- 将 `mt.exe` 所在文件夹加入**白名单 / Trust Zone**

### 情况四 / Case 4：以上均无效 — 关闭智能应用控制 / Disable Smart App Control

> 这是 Windows 11 特有的最严格拦截机制，按以下步骤关闭：
> *This is Windows 11's strictest blocking mechanism. Follow these steps to disable it:*

1. 打开 **Windows 安全中心** / Open **Windows Security** (shield icon in taskbar)
2. 进入 **应用和浏览器控制 / App & browser control**
3. 点击 **智能应用控制设置 / Smart App Control settings**
4. 将模式改为 **"关闭" / "Off"**
5. 重新双击 `mt.exe` 运行

> 若仍无法运行，右键 `mt.exe` → **以管理员身份运行 / Run as administrator**

---

## 购买激活 / Purchase & Activate

1. 点击软件右上角 **"获取满血版"** / Click **"Get Full Version"** in the top-right
2. 付款后系统自动通过爱发电私信发送激活码 / Code sent automatically via 爱发电 message after payment
3. 软件 → **激活** 标签页 → 输入激活码 → 立即激活 / Go to **Activate** tab → enter code → Activate

续费时输入新激活码，有效期自动从原到期日顺延 1 年。
*For renewal: enter the new code and the license extends 1 year from the original expiry date.*

---

## 更新日志 / Changelog

### v1.1.0
- **新增界面语言**：新增西班牙语（ES）和法语（FR），现支持 5 种界面语言（简中/繁中/英/西班牙/法语）
- **即时互译错误提示升级**：区分网络连接失败、API Key 无效、请求超时、频率限制四种错误类型，右侧结果框显示详细说明，错误信息随界面语言同步变化
- **切换 API 平台自动重新翻译**：在 API 设置页切换平台后，即时互译框内容自动用新平台重新翻译
- **状态提示永远可见**：修复窗口缩小时底部状态提示被遮挡的问题，调整布局确保始终可见
- **悬浮提示**：鼠标悬停在底部状态栏上，弹出浮窗显示完整错误内容

*New in v1.1.0:*
- *UI language: added Spanish (ES) and French (FR) — now 5 UI languages supported*
- *Live translation error messages: distinguishes connection failure, invalid API Key, timeout, and rate limit — details shown in the result box, language follows UI setting*
- *Auto-retranslate after switching API platform*
- *Status bar always visible — fixed layout issue where the status was hidden on small windows*
- *Tooltip on hover: hover over the status bar to see the full error message*

### v1.0.7
- **API 设置页交互优化**：点击任意平台行即自动切换当前平台；每行前新增单选框；标题处实时显示「启用·平台名」
- **价格展示优化**：激活页续费价格旁新增原价划线（~~¥1980/年~~）
- 移除顶部冗余的翻译平台切换模块，界面更简洁

### v1.0.6
- 价格统一调整为 ¥599/年

### v1.0.5
- 封面生成全面升级：新增描边颜色、修复多行标题居中、新增背景色带选项
- 标题与简介分隔：空行前为封面标题，空行后为简介
- 修复激活状态在更新版本后丢失的问题

### v1.0.4
- 全新封面生成功能：本地渲染秒级输出 + AI API 优化文案后渲染
- 支持字号、文字颜色、标题位置、横/竖屏尺寸

### v1.0.3
- 新增自定义接口地址（高级设置）
- 新增 AI 智能生成标题

### v1.0.2
- 新增标题翻译页面
- 即时互译新增快捷键：Ctrl+C 智能复制、Alt+D 清空

### v1.0.1
- 初始发布 / Initial release
