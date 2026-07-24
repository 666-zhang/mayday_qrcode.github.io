# 五月天主题 · 书籍推荐二维码

基于 GitHub Pages 的静态网页工具，为朋友生成带有五月天主题的个性化书籍推荐二维码卡片。扫码即可看到推荐理由、祝福语、歌词，并伴随五月天歌曲背景音乐自动播放。

---

## 效果展示

微信扫描下方二维码，即可跳转至对应书籍的推荐页面：

| Book 1 | Book 2 | Book 3 | Book 4 |
|:---:|:---:|:---:|:---:|

每张卡片包含：

- **书籍信息**：书名、作者、推荐理由
- **祝福语**：双行排版，空格处自动换行
- **歌词**：五月天歌曲歌词选段
- **BGM**：对应歌曲后台自动播放
- **卜卜 IP 形象**：纯 SVG 绘制，浮动呼吸动画

---

## 工作原理

```
生成二维码（Python）        →      扫码       →      GitHub Pages
将一个极短 URL 编码成 QR 码      微信内置浏览器打开      返回静态 HTML 页面
```

### 为什么用 `#` Hash 传参？

微信内置浏览器会截断 `?` 后的 Query String，导致参数丢失。改用 URL Hash（`#id=book1`）可以可靠地在微信环境下传递书籍 ID。

### 为什么 URL 这么短？

为保证 4cm × 4cm 小尺寸打印后仍可被微信顺利识别，URL 采用极简 ID 模式（约 50 字符），而非直接编码所有中文数据（500+ 字符）。书籍内容预先内嵌在 HTML 的 JS 对象中，扫码后根据 ID 查找对应数据渲染页面。

---

## 文件结构

```
mayday_qrcode/
├── index.html              # 核心模板页面（GitHub Pages 托管）
├── books.xlsx              # 书籍数据源（title/author/recommend/blessing/lyric/song/bgm）
├── batch_generate.py       # 批量生成二维码图片
├── generate_word.py        # 生成 A4 Word 打印文档
├── output/                 # 输出的二维码图片（book1~4.png）
├── 五月天二维码.docx        # A4 打印排版文档（4cm × 4cm）
├── *.mp3                   # 五月天歌曲背景音乐
└── README.md
```

---

## 本地使用

### 1. 生成二维码

```bash
python batch_generate.py
```

会根据 `books.xlsx` 中的书籍数据，为每本书生成一张 820×820 像素的二维码图片，输出到 `output/` 目录。

### 2. 生成打印文档

```bash
python generate_word.py
```

将所有二维码排列到 A4 纸张上（2 行 × 3 列，每张 4cm × 4cm），便于直接打印剪裁。

### 3. 新增/修改书籍

编辑 `books.xlsx`，添加新行或在 `index.html` 的 `BOOKS` 对象中新增对应条目，然后重新运行脚本即可。

---

## 技术栈

| 层级 | 技术 |
|------|------|
| 前端 | HTML5 + CSS3 + Vanilla JS |
| 二维码 | Python `qrcode` 库 |
| 排版 | `python-docx`（Word 文档） |
| 数据 | `openpyxl`（Excel 读写） |
| 部署 | GitHub Pages（免费静态托管） |

---

## 许可

个人项目，仅供朋友间分享使用。
