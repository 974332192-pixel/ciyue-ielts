# 词阅 · IELTS Reading Studio

> 一个为雅思考生设计的「沉浸式阅读 + 智能生词本」工具。**用文章本身做生词本**——重点词高亮、悬停看释义、一键收藏，LLM 兜底解释，告别背单词脱离语境。

![demo](https://img.shields.io/badge/demo-react_spa-blue) ![stack](https://img.shields.io/badge/stack-Vite_+_React_+_TS-purple) ![llm](https://img.shields.io/badge/llm-DeepSeek_V4_Flash-4d6bff) ![search](https://img.shields.io/badge/search-博查-00a86b) ![style](https://img.shields.io/badge/style-米黄琥珀色-paper)

---

## ✨ 这是什么

**词阅（IELTS Reading Studio）** 解决一个传统生词本工具的痛点：**背单词脱离语境**。

传统做法：背单词 App → 抄生词 → 第二天忘了怎么用。
**词阅做法**：读一篇雅思风格文章 → 看到生词直接悬停看释义 → 喜欢的句子一键收藏 → 收藏夹自动变成"生词本"。

适合：
- 🎓 雅思考生（阅读 6.0 想冲到 7.5+）
- 📚 喜欢精读英文文章的人
- 🧠 想用「语境记忆法」替代「死记硬背」的英语学习者

---

## 🚀 快速开始

**这是构建后的部署版本（Vite 输出）**，所以是**纯静态文件**：

```bash
# 方式 1：直接双击 index.html
# （注意：fetch 联网搜索可能受 file:// 协议限制）

# 方式 2（推荐）：起个静态服务
cd ciyue/
python3 -m http.server 8000
# 访问 http://localhost:8000
```

打开后**首次会要求配置 API Key**（页面设置面板里）：
- DeepSeek Key（必填，用于 LLM 解释）
- 博查 Key（可选，用于联网搜最新例句）

> 💡 Key 只保存在浏览器 localStorage，**不上传任何服务器**。

---

## 🎯 核心功能

### 1. 沉浸式阅读
- 内置多篇雅思风格文章（科技、认知、教育等话题）
- 重点词自动**高亮**（淡黄色背景）
- 鼠标悬停句子 → 整句变浅黄（**精准反馈**）
- 选中句子 → 整句变深黄

### 2. 智能生词本
- 点击高亮词 → 弹出 modal 显示：
  - 音标（IPA）
  - 词性
  - 英文释义
  - 中文释义
  - 例句
- 点击收藏按钮 → 整句入库
- 收藏夹就是你的生词本（**附带原文上下文**）

### 3. LLM 兜底解释
- 文章里没预设的生词？让 LLM 现场解释
- **流式输出**（逐字显示，体验更自然）
- 模型：`deepseek-v4-flash`（速度快、成本低）

### 4. 联网搜索（可选）
- LLM 解释不够？自动联网搜最新例句
- 用博查 API（DeepSeek 官方同款）

### 5. 状态持久化
- 文章、收藏、modal 位置、窗口大小 → **全部存 localStorage**
- 刷新页面不丢

---

## 🎨 设计语言

**米黄 + 琥珀色**主题，致敬纸质书的阅读感：

| 颜色变量 | 值 | 用途 |
|---------|----|----|
| `--paper` | `#FAFAF7` | 主背景（米黄） |
| `--amber` | `#D4A843` | 主色调（琥珀） |
| `--amber-dark` | `#C49A38` | 按钮 hover |
| `--highlight` | `rgba(255, 220, 80, 0.35)` | 重点词高亮 |
| `--hover-sentence` | `#FFF8E0` | 句子悬停 |
| `--selected-sentence` | `#FFF0C8` | 句子选中 |

**字体**：`-apple-system, BlinkMacSystemFont, Segoe UI`（系统字体栈，加载快、不用打包字体文件）

**细节**：
- 句子悬停有过渡动画（`transition: background-color .2s ease-out`）
- 拖拽 modal 时全局禁选（`user-select: none`）防止选到外面
- 自定义 markdown 渲染样式（标题、引用、code block）

---

## ⚙️ 配置

页面设置面板里可以改：

| 字段 | 默认值 | 说明 |
|------|--------|------|
| `llmBaseUrl` | `https://api.deepseek.com/v1` | LLM API 入口 |
| `llmKey` | 必填 | DeepSeek API Key |
| `llmModel` | `deepseek-v4-flash` | 推荐用 Flash 系列（快+便宜） |
| `searchUrl` | `https://api.bochaai.com/v1/web-search` | 博查 API |
| `searchKey` | 可选 | 博查 API Key |
| `enableLLM` | `true` | 是否启用 LLM 解释 |
| `enableSearch` | `true` | 是否启用联网搜索 |

**Token 消耗**：单次生词解释约 **300-500 tokens**（输入 ~200 + 输出 ~300），新用户免费额度够用。

---

## 📂 项目结构

```
ciyue/
├── index.html                      # 入口 HTML
├── assets/
│   ├── index-ClyN-Fs-.js          # JS bundle (Vite 打包，含 React + 业务代码)
│   └── index-CSxK5eOR.css         # CSS bundle（设计系统 + 组件样式）
└── README.md
```

**这是 Vite 构建产物**，不是源码。要二次开发需要：
1. 找原始 Vite 项目（`src/` + `package.json` + `vite.config.ts`）
2. `npm install` → `npm run dev`

如果只是想部署：把这三个文件扔到任何静态托管就行（Vercel / Netlify / Cloudflare Pages / GitHub Pages / OSS）。

---

## 🧠 一些设计决策

### 1. 为什么用 React + Zustand 而不是纯 HTML？
- 阅读界面有**复杂的交互状态**（悬停、选中、modal 拖拽、收藏）
- 状态分散在 5+ 维度，需要集中管理
- Zustand 比 Redux 轻 10 倍，bundle 只增加 ~1KB

### 2. 为什么高亮词用淡黄色而不是下划线？
- 下划线会**打断阅读节奏**（眼睛要去找下划线）
- 淡黄色背景**融入文本**，一眼能扫到但不影响理解
- 选中状态用深黄色 → 视觉层级清晰

### 3. 为什么 LLM 用 Flash 系列而不是 V3？
- 单次解释任务 V3 杀鸡用牛刀
- Flash 速度 2-3 倍，**流式输出更跟手**
- 雅思生词解释不需要复杂推理

### 4. 为什么不预设所有生词的释义？
- 词典释义是死的，**LLM 解释能结合文章上下文**
- 覆盖长尾词成本太高
- 流式 LLM 也能当"教学"，展示完整释义结构

---

## 🔧 二次开发指引

> 这一节需要**原始 Vite 源码**才有用。源码不在本仓库里。

如果你拿到的只是构建产物（`index.html` + `assets/`），能改的有限：
- **改文案**：直接编辑 `assets/index-*.js` 里的字符串（搜中文）
- **改样式**：直接编辑 `assets/index-*.css`
- **改配置默认值**：搜 `ielts-studio-config` 找到默认 JSON，改 key 字段
- **二次构建**：拿不到源码就别想了

如果有源码，常见改动点：
- 换模型：搜 `deepseek-v4-flash` 改模型名
- 加新文章：编辑 `du` 对象（示例文章数据）
- 改 LLM prompt：搜 `messages` 找到 prompt 模板

---

## 🐛 已知问题 / 待优化

- [ ] **file:// 协议下 fetch 受限**：直接双击 index.html 打开，部分 LLM/搜索 API 会失败（CORS）。必须用本地 server
- [ ] **收藏夹没分组**：收藏多了会乱，没法按"科技/教育/认知"分类
- [ ] **导出功能缺失**：收藏夹没法导出成 Anki / 欧路词典格式
- [ ] **没有复习曲线**：背了就忘，没用上 SM-2 / FSRS 这类间隔重复算法
- [ ] **移动端体验一般**：modal 拖拽是为桌面设计的，手机上不友好

---

## 🧪 面试 Demo 用法

30 秒讲清楚这个产品的设计：

1. **打开主页** → 展示文章列表
2. **演示重点 A**：打开一篇 → 悬停高亮词 → 弹窗显示释义
3. **演示重点 B**：点击收藏 → 进收藏夹 → 展示"生词本"形态
4. **演示重点 C**：解释流程 → 演示 LLM 流式输出（如果有联网搜索更好）
5. **结尾**：强调"语境化生词本" vs "传统背单词 App"的差异

如果面试官问"为什么用 LLM 解释，不用词典 API"：
> "词典 API 是死的，LLM 能结合文章上下文给解释。比如 'catalyze' 在科技文章里是'催化'，在社会文章里是'促进'，词典都给'催化'就丢了语义。这是**工具型 AI vs 信息型 AI**的差异——AI 能理解场景。"

---

## 📜 License

MIT

## 🙏 致谢

- DeepSeek（主力 LLM，便宜好用）
- 博查 BochAI（联网搜索）
- Vite + React + Zustand（前端三件套）
- 灵感来自《把时间当作朋友》——「语境记忆法」
