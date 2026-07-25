# 海狸大師部落格

Hugo 部落格，使用 `hugo-theme-stack` 主題。

## 常用指令

- 新增中文文章（預設語言）：
  ```bash
  hugo new content post/<slug>.md
  # 或明確指定
  hugo new content content/zh-tw/post/<slug>.md
  ```

- 新增英文文章：
  ```bash
  hugo new content content/en/post/<slug>.md
  ```

- 本機預覽：
  ```bash
  hugo server -D
  ```

- 建立網站：
  ```bash
  hugo --gc --minify
  ```

- 更新 Hugo：
  ```bash
  brew upgrade hugo
  ```

- 更新主題：
  ```bash
  cd themes/hugo-theme-stack
  git fetch --tags
  git checkout v4.0.3  # 或最新的穩定版
  ```

## 小提醒

- 路徑要加 `.md`；想要 page bundle 就用 `.../<slug>/index.md`。
- `hugo-theme-stack` v4 的 layout 已改為 `layouts/_partials/`、`layouts/_shortcodes/`、`_markup/`，覆寫主題時要放對路徑。
- `timeout` 設為 5 分鐘，因為站內有些遠端圖片需要較長處理時間。
