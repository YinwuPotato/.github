# Yinwu 插件系列

为 **Yinwu 服务器群组**开发的自制 Minecraft 服务端插件：后端服（Paper / Folia）、Van 服（Canvas）、
以及 Velocity 代理。全部按平台分仓库维护。

> 所有插件均以 **Folia 区域线程安全**为设计前提（`RegionScheduler` / `GlobalRegionScheduler` /
> `EntityScheduler`，不使用 `Bukkit.getScheduler()`），并且只使用 Paper / Folia 公共 API。

---

## Sur —— 后端服（Paper / Folia 1.21.x）

| 插件 | 说明 |
|---|---|
| [YinwuPluginLib](https://github.com/YinwuPotato/YinwuPluginLib) | **共享库**：插件基类、Folia 调度封装、跨插件 API 接口、GUI 框架、物品/多语言/配置工具。是其它 Sur 插件的**编译期前置**（构建产物已把它 shade 进去，服务器上无需单独安装） |
| [YinwuForge](https://github.com/YinwuPotato/YinwuForge) | 锻造系统：药水锻造 + 属性强化、多层锻造祭坛、30 种浓缩材料、收益递减 |
| [YinwuRaid](https://github.com/YinwuPotato/YinwuRaid) | 灾厄袭击：倒置信标、灾厄之种、多波次袭击（精英 / Boss）、14 职业村民奖励 |
| [YinwuEnchant](https://github.com/YinwuPotato/YinwuEnchant) | 34 个自定义附魔（PDC 存储）、单物品附魔开关 GUI、Deeper Dark 附魔拦截 |
| [YinwuFlightBlock](https://github.com/YinwuPotato/YinwuFlightBlock) | 飞行方块：范围内非创造 / 非旁观玩家自动获得飞行 |
| [YinwuLlamaGuard](https://github.com/YinwuPotato/YinwuLlamaGuard) | 羊驼防卫：玩家 32 格内的羊驼自动攻击范围内的幻翼（原版机制与伤害不变，只是吐得更勤）；带每只羊驼独立的开关与战绩统计 |
| [YinwuTax](https://github.com/YinwuPotato/YinwuTax) | 税务插件：为服务器提供更加清晰、灵活的税务管理能力。插件围绕经济系统运行，支持按配置自动结算税单） |
| [YinwuLottery](https://github.com/YinwuPotato/YinwuLottery) | 酒方抽奖：6×9 箱子转盘（可配置减速停止、音效与动画），需要 BreweryX 提供酒方、Vault / VaultUnlocked 提供经济 |
| [ShootEXP-folia](https://github.com/YinwuPotato/ShootEXP-folia) | 经验射击（上游 [ShootEXP](https://github.com/qumingjam/ShootEXP-folia) 的 Folia 分支） |

## Van —— Van 服（Canvas 26.3 / Folia 区域线程）

| 插件 | 说明 |
|---|---|
| [YinwuServux](https://github.com/YinwuPotato/YinwuServux) | 在 Canvas 上实现 masa 的 `servux:entity_data` 协议 → MiniHUD 的**容器预览**、**村民信息与交易显示**（含可选的客户端补丁 YinwuVillagerPin 与 Velocity 版本中继） |
| [YinwuVaultFix](https://github.com/YinwuPotato/YinwuVaultFix) | 让试炼宝库"开过就不再给"的玩家黑名单失效，同一个宝库可反复开启 |
| [TpsBarAlias](https://github.com/YinwuPotato/TpsBarAlias) | 把 `/tpsbar` 变成 Canvas 内置命令 `/regionbar tps_bar` 的别名（不绕过权限） |
| [YinwuVillagerDiscount](https://github.com/YinwuPotato/YinwuVillagerDiscount) | 共享村民折扣：任何人治愈僵尸村民后，那份折扣对全服玩家生效（移植 totos-carpet-tweaks 的 `sharedVillagerDiscounts`） |

## Velocity —— 代理

| 插件 | 说明 |
|---|---|
| [YinwuLastServer](https://github.com/YinwuPotato/YinwuLastServer) | 记住玩家上次所在的子服，进服直接送回；目标服不可用则回退默认大厅，转服失败不把玩家踢出代理 |

---

## 安装顺序

1. **代理侧**：`YinwuLastServer` 放进 `velocity/plugins/`。
2. **`YinwuServux` 与 `velocity-客户端版本中继` 必须成对使用** ——
   只装后端则跨版本客户端握手失败，只装中继则后端收不到版本串。
3. 其余插件各自独立：把 jar 丢进对应服的 `plugins/` 即可。

> **不需要单独安装 `YinwuPluginLib`**：Sur 系各插件的构建产物已经把该库 shade 进自己的 jar
> （`YinwuForge` / `YinwuRaid` / `YinwuEnchant` / `YinwuFlightBlock` / `YinwuLlamaGuard`，以及 Van 的
> `YinwuVillagerDiscount` 都是如此）。它只是**编译期**前置。

### 自行编译时

每个仓库都自带 `parent/pom.xml`（父 POM），但 `YinwuPluginLib` 不在 Maven 中央仓库 ——
首次构建请先在它仓库里 `mvn clean install` 装进本地仓库，之后其它插件才能 `mvn clean package`。

各插件的配置、命令、验收步骤见各自仓库的 `README.md`；发布包见各仓库的 **Releases**。
