# Project SENTINEL SurveyOps

**自助结账调查数据工作台** —— 面向超市自助结账系统现场调查（45 分钟/场，角色A + 角色B 双观察员）的本地数据录入、校验、计算、诊断、总结与导出工具。

> 目标：除场次名称、必要备注等少量信息外，使用者最多只负责录入数字、选择代码、时间和选项。所有求和、比率、异常检测、跨表核对、图表、问题诊断和报告都由程序完成。

## 📄 单文件版（国内可用，推荐首选）

**`sentinel-surveyops-单文件版.html`** —— 整个应用打包成一个 HTML 文件，**零服务器、零实名、零注册、不依赖任何网络**：发给任何人（微信/QQ/网盘），对方双击文件直接用。**这是国内网络下开箱即用的形态**。详见 [docs/单文件版.md](docs/单文件版.md)。

> ⚠️ 海外网址 `https://sentinel-surveyops.surge.sh` 实测被国内运营商 **HTTP 451 拦截**，**国内网络打不开**（仅限海外或可翻墙环境）。国内需要网址的，请走 [docs/国内部署.md](docs/国内部署.md)（腾讯云 COS 实名一次）。

## 特性一览

- **本地优先**：数据全部保存在浏览器 IndexedDB，离线可用，刷新不丢失；无服务器、无账号、无收费 API。
- **发给任何人就能用**：构建产物是纯静态网站，部署到免费托管（GitHub Pages / Netlify Drop / Cloudflare Pages）即得**永久 https 网址**，手机/电脑任意浏览器打开即用（见 [docs/永久部署.md](docs/永久部署.md)）。
- **链接直传**：单场/全量数据压缩后编进网址（数据在链接里，永不过期），把链接发给任何人，对方打开 → 一键导入 → 直达数据。
- **跨设备**：PWA 可安装到手机/电脑；JSON 备份 + 系统分享/剪贴板/二维码传输，另一台设备导入即合并（按场次编号去重）。
- **完整数据模型**：场次信息 + 角色A（A1 存量 / A2 九个五分钟段 / A3 人工旅程抽样）+ 角色B（B1 自助区状态 / B2 九段 / B3 自助旅程抽样 / B4 事件表）。
- **严格缺失值体系**：`0`（确认没发生）与 空白（未录入）、`UNK / NA / MSS / R` 严格区分，程序绝不把空白或代码当作 0；录入界面用按钮/下拉选择，无需手写代码。
- **四级校验**：红（确定错误）/ 黄（待确认异常）/ 蓝（提示）/ 灰（数据不足），20 条内置规则；红色问题必须修正或填写强制覆盖理由；已锁定场次必须先解锁并记录原因；所有修改写入审计日志。
- **自动指标**：每项指标附公式说明、分子、分母、有效样本量、缺失比例、可信度（准确/近似/证据不足）；缺失不填补、不编造。
- **诊断中心**：数据质量问题（A 类）与门店运营问题（B 类）完全分开；只给"可能解释"，证据不足时明确显示"当前不能判断"。
- **导入导出**：单场 JSON、全量 JSON 备份/恢复、CSV（每表一个文件）、XLSX（含校验问题工作表）、A4 打印/导出 PDF；数据结构版本号 + 迁移机制。
- **内置演示数据**：高峰完整正确 / 低峰 / 大量 MSS 与遮挡 / 故意含错误（T≠I+B+K+O、MB>MO、支付时间倒置、重复样本ID、PF 与事件表冲突、空白与 0 混用）。

## 安装与启动

需要 Node.js ≥ 20（开发环境建议 22+）。

```bash
npm install
npm run dev          # 开发模式，默认 http://localhost:5173
npm run build        # 生产构建（含 PWA 离线缓存）
npm run preview      # 本地预览构建产物
.\deploy.ps1         # 一键构建 + 部署准备（-ServeLan 启动局域网服务）
```

**给任何人用（永久网站）**：`npm run build` 后把 `dist/` 拖到 Netlify Drop（https://app.netlify.com/drop）即得永久 https 链接；或按 [docs/永久部署.md](docs/永久部署.md) 用 GitHub Pages / Cloudflare Pages。部署后手机、电脑、任意浏览器打开链接直接使用，配合「链接直传」分享具体场次数据。

详细说明见 [docs/安装与启动.md](docs/安装与启动.md)。

## 测试

```bash
npm test             # 单元测试（vitest，78 个用例）
npm run test:e2e     # 浏览器端到端测试（Playwright，桌面 + 手机宽度）
npm run typecheck    # TypeScript 类型检查
```

详见 [docs/测试方法.md](docs/测试方法.md)。

## 文档索引

| 文档 | 内容 |
| --- | --- |
| [用户操作说明](docs/用户操作说明.md) | 普通用户：如何开始一场调查、录入、检查、确认、导出 |
| [永久部署](docs/永久部署.md) | 部署为永久网站（发给任何人打开即用）的三种免费方式 |
| [数据字段字典](docs/数据字段字典.md) | 场次 / A1 / A2 / A3 / B1 / B2 / B3 / B4 全部字段与代码 |
| [指标口径和计算说明](docs/指标口径和计算说明.md) | 每个指标的公式、分子分母、可信度规则 |
| [校验规则清单](docs/校验规则清单.md) | 20 条校验规则（R1–R20）与四级颜色 |
| [测试方法](docs/测试方法.md) | 单测与 e2e 覆盖范围、运行方式 |
| [已知限制](docs/已知限制.md) | 当前版本已知限制与使用注意事项 |
| [后续可扩展方向](docs/后续扩展方向.md) | 未来版本规划 |

## 技术栈

React 18 + TypeScript（strict）+ Vite 6 + react-router（Hash 路由）+ zustand + IndexedDB（idb）+ recharts + SheetJS（xlsx）+ PapaParse（CSV）+ qrcode + vite-plugin-pwa + vitest + Playwright。

## 目录结构

```
src/
  types/model.ts       # 数据模型与缺失值体系
  lib/                 # codes / time / storage / validation / metrics / diagnosis / summary / importexport / demo / ids / collect
  store/appStore.ts    # zustand 全局状态 + 自动保存
  components/          # 共享 UI 组件
  pages/               # 10 个页面
tests/e2e/             # Playwright 浏览器测试
docs/                  # 文档
```

## 许可证

MIT
