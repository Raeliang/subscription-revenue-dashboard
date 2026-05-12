# Subscription Revenue Dashboard

一个可直接部署到 GitHub Pages 的静态 Subscription Revenue Dashboard。页面只需要 `index.html` 和同目录数据文件，不需要后端和构建步骤。

## 文件

- `index.html`：完整单文件 Dashboard，HTML/CSS/JavaScript 内嵌业务逻辑。
- `data.csv`：当前真实数据，已从 `/Users/rrrae/Downloads/subscription_dashboard_ready.xlsx` 的 `clean_subscription_data` sheet 导出。
- `data.example.csv`：字段示例。
- `README.md`：使用说明。

## 数据字段

CSV 推荐字段：

```csv
Date,Country,Platform,Subscription_Type,Revenue_EUR,Orders
2026-04,US,Apple,Monthly,248.30,262
2026-04,ALL,Google,Unknown,39157.33,
```

Apple 支持国家、订阅类型和订单数。Google 当前只有收入时，可以把 `Country` 写成 `ALL`，`Subscription_Type` 写成 `Unknown`，`Orders` 留空。

## 每月更新数据

1. 打开原始 Excel。
2. 确保主数据 sheet 包含这些列：`Date`、`Country`、`Platform`、`Subscription_Type`、`Revenue_EUR`、`Orders`。
3. 导出为 CSV，并命名为 `data.csv`。
4. 替换仓库里的 `data.csv`。
5. 刷新网页，所有 KPI、图表、表格会自动重新计算。

页面加载顺序：

1. 优先读取同目录下的 `data.csv`。
2. 如果找不到 CSV，会尝试读取 `subscription_dashboard_ready.xlsx`。
3. 如果浏览器或网络环境无法读取 Excel，页面会提示导出为 CSV。

## 本地预览

由于浏览器对 `file://` 下的 `fetch` 有限制，建议用本地静态服务器预览：

```bash
cd /Users/rrrae/subscription-revenue-dashboard
python3 -m http.server 4173
```

然后打开：

```text
http://localhost:4173
```

## 部署到 GitHub Pages

1. 新建 GitHub 仓库。
2. 上传至少这两个文件：
   - `index.html`
   - `data.csv`
3. 进入 GitHub 仓库的 `Settings`。
4. 打开 `Pages`。
5. Source 选择 `Deploy from a branch`。
6. Branch 选择 `main` 和 `/root`。
7. 保存后等待 GitHub Pages 生成公开链接。

不需要 Node、npm、构建命令或后端服务。

## 替换 Excel

如果你希望直接上传 Excel：

1. 把 Excel 命名为 `subscription_dashboard_ready.xlsx`。
2. 与 `index.html` 放在同一目录。
3. 保留 `clean_subscription_data` sheet，或至少保留包含 `Date`、`Platform`、`Revenue_EUR` 的 sheet。

更稳定的生产方式仍然是使用 `data.csv`。

## 修改主题颜色

打开 `index.html`，搜索：

```js
const COLORS = {
```

可以修改主要图表颜色。

同时可以搜索 CSS 变量：

```css
:root {
  --page: #f8fafc;
  --accent: #3b82f6;
}

.dark {
  --page: #0b1020;
}
```

这些变量控制页面背景、卡片、边框、文字和深色模式。

## 依赖说明

`index.html` 已内嵌浏览器端依赖：

- TailwindCSS
- ECharts
- SheetJS，用于浏览器读取 `.xlsx`

GitHub Pages 可直接运行，也可以在无外网环境下通过本地静态服务器运行。唯一需要保持外部文件的是业务数据文件 `data.csv`，这样后续每月只替换数据即可。
