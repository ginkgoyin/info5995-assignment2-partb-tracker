# Part B 非登录批量筛查流程

日期：2026-05-08

## 目的

这份文档记录当前阶段采用的 `Part B` 实际执行流程。  
它覆盖的是“**不需要登录即可完成的检查项**”的批量筛查模式，用于在所有目标上先完成一轮稳定、可比较、可复用的非登录检查。

这份流程是对现阶段 `PARTB-README.md` 的执行补充：
- `PARTB-README.md` 仍然是总体方法与评分对齐文档
- 本文档是当前阶段的**实际运行规则**

## 当前执行模式

当前不再采用“一个项目先做登录前，再停下来等人工登录，再继续”的模式。  
当前采用的是：

1. 根据 `partb/target-selection.md` 的目标清单，从前往后逐个项目推进。
2. 对每个项目，先完成所有**非登录必须检查点**。
3. 如果在非登录阶段发现漏洞或强候选 finding：
   - 记录详细技术细节
   - 保留必要证据
4. 如果在非登录阶段没有发现漏洞：
   - 只在 `partb/master-tracker.md` 中做简要记录
   - 自动进入下一个项目
5. 全流程自动推进，不需要中途确认，直到所有项目的非登录必须检查点都完成。
6. 每个项目都必须主动查看源码与调试面板，而不是只看 UI：HTML、内联脚本、加载的 JS/CSS、XHR/fetch、hidden params、token、nonce、state、对象 ID、暴露路由，以及可疑随机数/绑定行为。
7. 每个项目结束后，要关闭或清理掉专门为该项目打开的浏览器标签页，避免残留无关测试页面。

## 记录规则

### 有漏洞 / 强候选 finding

满足以下任一条件时，记录详细技术细节：
- 已发现明确 `vulnerability`
- 已出现强候选 finding，值得后续提交或深入验证
- 已获得高价值证据，足以支持后续 presentation/Q&A

此时应保留：
- 精确目标 URL
- scope 证明
- 请求/响应
- 参数、对象 ID、token、nonce、state 等
- 截图
- 影响说明
- severity / novelty 初步判断

### 无漏洞

如果没有发现漏洞，则：
- **不要**写详细技术日志
- **只**在 `partb/master-tracker.md` 中简要记录：
  - 平台
  - program
  - program bounty URL
  - 官方网站
  - 完成了哪些非登录检查项
  - 结果：`No finding`

## 每个项目必须完成的非登录检查清单

每个项目在进入下一个项目之前，必须确认下面这些检查项已经完成。

### 1. Program intake / scope lock

必须完成：
- 读取 program page
- 确认 bounty URL
- 确认官方站点
- 确认该项目来自老师允许的平台或明确允许的 company-run program
- 确认没有明显超 scope 的测试意图

### 2. Attack surface mapping

必须完成至少一轮静态映射，识别：
- 首页
- 登录入口
- 注册入口
- 重置密码入口
- 邀请 / 分享 / onboarding
- developer / docs / API / billing / account 面
- 其他显然带对象边界的页面

### 3. Pre-auth UI review

必须检查：
- 是否存在公开对象页面
- 是否存在公开分享链接
- 是否存在可疑参数
- 是否存在反射点
- 是否存在公开下载/预览/嵌入对象
- 是否存在未登录可访问的业务流程页面

### 4. Pre-auth DevTools deep dive

必须做 DevTools 级检查，不能只看页面。

至少要查看：
- HTML / 内嵌状态
- JS / bundle
- CSS 不是重点，但如涉及状态切换可辅助观察
- XHR / fetch
- object IDs
- token / nonce / state / csrf
- hidden params
- exposed routes / endpoints
- 登录前即可触发的重要请求
- 重复流程中的随机参数、一次性参数、绑定参数是否复用或异常稳定

### 5. 五类漏洞基线检查（仅非登录部分）

#### 5.1 Access control / IDOR / object exposure

必须检查：
- 公共对象 URL
- 可枚举对象 ID
- 分享链接
- 嵌入链接
- 下载/预览链接

#### 5.2 Auth / session / invite / reset / verification / OAuth

必须检查：
- 登录入口参数
- 注册流程参数
- reset / verify / invite / OAuth 入口
- redirect / return URL
- state / nonce / code_challenge 是否出现

#### 5.3 Token / nonce / randomness / binding quality

必须检查：
- token 是否暴露在页面或请求中
- nonce / state 是否明显可预测或复用
- token 是否只依赖容易猜测的对象
- 是否存在可疑短 token / 结构化 token

#### 5.4 Input handling / XSS / unsafe reflection

必须检查：
- 搜索框
- query 参数
- 登录/注册前公开输入框
- 公开反馈/联系表单
- 反射型输入点

#### 5.5 Misconfiguration / unsafe exposure / business logic boundary

必须检查：
- 暴露调试信息
- feature flags
- 公开 API 路由
- 业务流程暴露
- 公开的敏感前端状态

## 工具要求

本阶段允许并鼓励使用：
- Chrome DevTools
- Chrome MCP
- 浏览器源码查看
- XHR/fetch 请求观察
- object ID / token / state / hidden param 检查

默认原则：
- 能进请求层就不要只停在 UI 层
- 能验证参数/对象边界就不要只描述页面存在
- 非登录阶段默认先做低风险、低频、定向验证

## 切换到下一个项目的条件

只有当以下条件都满足时，才自动进入下一个项目：

- program/scope 已确认
- 攻击面已映射
- pre-auth UI review 已完成
- pre-auth DevTools deep dive 已完成
- 五类非登录基线检查已完成
- 已确认“有漏洞并详细记录”或“无漏洞并简记入 master-tracker”

## 当前批处理起点

当前按 `partb/target-selection.md` 的推荐顺序执行，起始队列为：

1. `Airtable`
2. `Shopify`
3. `Basecamp`
4. `TripAdvisor`
5. `GitHub`

后续再按扩展目标池继续推进。
