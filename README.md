# 技术瓜内盘快买 · Tech Melon FastBuy

<img src="logo.svg" width="96" alt="技术瓜内盘快买" />

基于 **Binance Agentic Wallet**（币安官方 `@binance/agentic-wallet` CLI）的本地交易控制台 + 浏览器插件。
粘贴或在网页上识别一个 BSC 合约地址（CA），自动判断它在哪个平台，点一下金额就买、点一下比例就卖：

- **内盘**：Four（OpenFour）、gang.fun（LaunchpadV2 / V3）、Flap（曲线市场与新版 Portal 税币）；
- **外盘**：代币毕业后自动走 PancakeSwap SmartRouter（V2 + V3 混合路由、单笔原子成交、冲击过大自动拒绝）。

交易都由币安钱包服务签名广播，**控制台和插件都不接触私钥 / 助记词**。

> 本项目不是币安官方产品。内盘新币风险很高（未审计、可能被抢先或归零），只用能承受损失的资金，先小额试用。

## 两个版本

包根目录的 `EDITION.txt` 写着你拿到的是哪一个。

| 功能 | 技术瓜内盘快买（轻量版） | 技术瓜内盘快买 **Pro**（全量版） |
|---|:---:|:---:|
| 交易台：粘贴 CA 自动识别平台、一键买卖、多钱包同时下单 | ✅ | ✅ |
| 直接买入（预授权后点金额免确认） | ✅ | ✅ |
| 持仓（链上余额 + GMGN 估值）、一键转移、发射后自动转移 | ✅ | ✅ |
| 报价币库存与授权、路由记录、Four / Flap 手动交易 | ✅ | ✅ |
| 兑换 / 转账 / 合约调用 / AI 执行 | ✅ | ✅ |
| 浏览器插件「技术瓜内盘快买 · 一键买卖」 | ✅ | ✅ |
| gang V2 / V3 内盘狙击 + 狙击看板 | — | ✅ |
| Four 新币狙击 | — | ✅ |
| gang 打新 / 自动打新、新部署发现 | — | ✅ |

## 快速开始（约 10 分钟）

需要：Windows 10/11、[Node.js](https://nodejs.org/) 18+（推荐 22 LTS）、开通了 Agentic Wallet 的币安 App。

1. **解压**到只有你自己能访问的目录，例如 `D:\fastbuy`。
2. **安装**：在解压目录打开 PowerShell，运行 `node scripts/setup.mjs`，菜单选 `1`。
   它会安装依赖、安装锁定版本的币安 CLI，并禁用 CLI 的系统钥匙串（否则多个钱包会串号）。
3. **启动**：双击 `webui\start.cmd`（或 `node webui\server.mjs`），浏览器打开 <http://127.0.0.1:8790>。
4. **登录钱包**：「系统 ▾ → 钱包」→ 添加钱包 → 登录 → 用币安 App 扫码，**核对配对码一致**后确认。
   需要合约交易的功能，请在币安 App 的 Agentic Wallet 设置里打开 **Developer Mode**。
5. **设上限**：「资金与授权 → 买入上限」填单笔 / 每日上限（例如 0.1 / 1 BNB）。直接买入、狙击等自动化都要求两项 > 0。
6. **（建议）配节点**：「系统 ▾ → 设置」填一个付费 BSC RPC（QuickNode / NodeReal 等）；用 Four 狙击还要填 BSC WSS。
7. **第一笔交易**：回到「交易台」，粘贴一个 CA，看到「识别：⭐ 平台名 · 阶段」后点金额，核对确认框里的每一步和上限，确认。
8. **（可选）装插件**：见下方「浏览器插件」。

做完这些就能用了。每一步的细节和所有配置项见 `DEPLOY.md`，各页面怎么用见 `docs/USAGE.md`。

## 浏览器插件

插件在任意网页（GMGN、four.meme、gang.fun、DexScreener、BscScan…）识别地址栏或刚复制的 CA，弹出可拖动的一键买卖窗口，
下单仍然走你自己的控制台。

1. Chrome / Edge 打开 `chrome://extensions`，打开「开发者模式」，「加载已解压的扩展程序」选择包里的 `extension` 文件夹；
2. 插件设置页会自动打开，复制里面的「插件 ID」；
3. 控制台「系统 ▾ → 设置 → 浏览器插件」填入插件 ID，点「生成配对密钥」；
4. 把密钥和控制台地址填回插件设置页，点「保存并测试连接」。

插件只能调用识别、报价、买卖、钱包、上限、持仓这几类接口；改设置、预授权、转账都只能在控制台里做。详见 `extension/README.md`。

## 文档地图

| 想做什么 | 看哪里 |
|---|---|
| 安装、启动、登录、所有配置项、公网部署、升级、常见问题 | `DEPLOY.md` |
| 每个页面怎么用、常用操作流程 | `docs/USAGE.md` |
| 项目怎么组织、一笔交易怎么走完、数据存在哪 | `docs/ARCHITECTURE.md` |
| 哪些操作必须逐笔确认、每种自动化的边界 | `docs/AUTOMATION_RULES.md` |
| 快捷交易、外盘路由、报价币库存、一键转移的细节 | `docs/QUICK_TRADE.md` |
| Four 狙击 / gang 新部署与打新（仅 Pro） | `docs/FOUR_SNIPER.md`、`docs/GANG_LAUNCH.md` |
| 安全模型、已知限制 | `docs/SECURITY_MODEL.md`、`docs/KNOWN_LIMITATIONS.md` |
| 手动调用币安 CLI | `docs/COMMANDS.md` |
| 插件 | `extension/README.md` |

## 安全要点

- 服务默认只监听 `127.0.0.1`；放到服务器上远程访问必须按 `DEPLOY.md` 第 12 节配 HTTPS、IP 白名单和登录密码。
- 每一笔交易都先经币安模拟（preview），再按你确认过的计划执行；自动化功能必须先确认一份有上限、有有效期的「预授权」。
- 顶部状态条的「⛔ 全部解除」一键关闭所有自动化。
- `webui/config.json`、`profiles/`、`webui/journal*.jsonl` 等是你的私人数据（节点令牌、API Key、登录会话、交易记录），不要发给任何人。

## 下载

| 文件 | 说明 |
|---|---|
| [`techmelon-fastbuy-lite-20261029-388046f.zip`](techmelon-fastbuy-lite-20261029-388046f.zip) | 控制台（轻量版），解压后从包里的 `README.md` 开始 |
| [`techmelon-fastbuy-extension-0.6.4-388046f.zip`](techmelon-fastbuy-extension-0.6.4-388046f.zip) | 浏览器插件，解压后「加载已解压的扩展程序」 |

每个 zip 附 `.sha256`：`certutil -hashfile <文件> SHA256`（Windows）或 `sha256sum <文件>`。Pro（全量版，含狙击 / 打新）不在这里公开发布。
