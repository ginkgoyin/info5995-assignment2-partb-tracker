# Part B 批量项目核查流程

日期：2026-05-09

## 目的

这份文档记录当前阶段 `Part B` 的实际执行流程。  
它是对 [C:\yx\go aboard\0-university of Sydney\26s1\INFO5995\00assignment2\PARTB-README.md](C:\yx\go%20aboard\0-university%20of%20Sydney\26s1\INFO5995\00assignment2\PARTB-README.md) 的运行层补充，用来指导我们如何按照统一标准，批量推进目标清单中的项目。

核心目标：
- 以“找到至少一个有效 vulnerability”为第一优先级。
- 对无漏洞项目保持轻量记录。
- 对真正的候选漏洞保留充分证据。
- 保证每个项目都完成源码、DevTools、请求层的必要检查，而不是停留在 UI 层。

## 当前执行模式

当前采用的是“自动批量推进”模式：

1. 根据 `partb/target-selection.md` 的目标清单，从前往后逐个项目推进。
2. 对每个项目，先完成所有登录前必须的检查项。
3. 如果登录前发现漏洞或强候选 finding：
   - 记录详细技术细节
   - 保留 scope、请求、响应、截图、影响说明等证据
4. 如果登录前没有发现漏洞：
   - 默认只在 `partb/master-tracker.md` 中简要记录
   - 自动进入下一个项目
5. 如果当前项目在主浏览器中已经是登录态：
   - 不需要停下来等待人工确认
   - 直接转入登录后检查
   - 继续按 `PARTB-README.md` 中的登录后核心检查执行
6. 整个流程默认自动推进，不需要中途确认，直到目标清单中的批量检查阶段完成。

## 登录态切换规则

后续统一按下面规则处理：

1. 优先以未登录态完成登录前检查。
2. 如果当前站点已经处于登录态，或者浏览器已有稳定登录会话：
   - 可以直接进入登录后检查
   - 不必强行停留在“仅非登录态”阶段
3. 如果主浏览器登录态会干扰未登录检查：
   - 优先使用访客窗口或独立窗口完成登录前检查
4. 如果已经完成登录前检查，且后续又发现现成登录态：
   - 可以立即补做登录后检查
   - 不必重新等待人工登录

## 记录规则

### 1. 有漏洞 / 强候选 finding

满足以下任一条件时，记录详细技术细节：
- 已发现明确 `vulnerability`
- 已出现强候选 finding，值得后续提交或深挖
- 已获得足够高价值证据，足以支撑 presentation / Q&A

此时应保留：
- 精确目标 URL
- program bounty URL
- scope 证明
- 请求/响应
- object ID、token、nonce、state、hidden params
- 截图
- 影响说明
- severity / novelty 初步判断

### 2. 无漏洞

如果没有发现漏洞：
- 不写详细技术长文档
- 只在 `partb/master-tracker.md` 中简要记录：
  - 平台
  - program
  - program bounty URL
  - 官方网站
  - 完成了哪些检查项
  - 结果：`No finding`

## 每个项目必须完成的检查清单

### 1. Program intake / scope lock

必须完成：
- 读取 program page
- 确认 bounty URL
- 确认官方网站
- 确认该目标属于老师允许的平台或明确允许的 company-run program
- 确认 scope 和规则

### 2. Attack surface mapping

必须至少完成一轮静态映射，识别：
- 首页
- 登录入口
- 注册入口
- 重置密码入口
- 邀请 / 分享 / onboarding
- developer / docs / API / billing / account 面
- 其他显然带对象边界的页面

### 3. 登录前 UI 检查

必须检查：
- 是否存在公开对象页面
- 是否存在公开分享链接
- 是否存在可疑参数
- 是否存在反射点
- 是否存在公开下载 / 预览 / 嵌入对象
- 是否存在未登录可访问的业务流页面

### 4. 登录前 DevTools 深查

必须做 DevTools 级检查，不能只看页面。

至少查看：
- HTML / 内嵌状态
- JS / bundle
- XHR / fetch
- object IDs
- token / nonce / state / csrf
- hidden params
- exposed routes / endpoints
- 登录前即可触发的重要请求
- 重复流程中的随机参数、一次性参数、绑定参数是否复用或异常稳定

### 5. 五类漏洞基线检查

#### 5.1 Access control / IDOR / object exposure

必须检查：
- 公共对象 URL
- 可枚举对象 ID
- 分享链接
- 嵌入链接
- 下载 / 预览链接

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
- 是否存在可疑短 token 或结构化 token

#### 5.4 Input handling / XSS / unsafe reflection

必须检查：
- 搜索框
- query 参数
- 登录前公开输入框
- 反馈 / 联系表单
- 反射型输入点

#### 5.5 Misconfiguration / unsafe exposure / business logic boundary

必须检查：
- 调试信息
- feature flags
- 公开 API 路由
- 业务流程暴露
- 敏感前端状态

## 如果检测到已登录态，追加执行的项目

一旦确认当前项目已有登录态，必须继续执行：

1. `Post-auth state confirmation`
   - 当前账号状态
   - 角色 / 权限
   - 已解锁导航
   - subscription / trial / account 状态
   - 新出现的 workspace / project / report / app / order / settings / developer 面

2. `Post-auth core checks`
   - 对真实对象 ID 做低频改值测试
   - 检查 session / token / invite / reset / OAuth 绑定
   - 检查 share / invite / collaboration / ownership 边界
   - 检查 plan / trial / billing gate
   - 检查登录后输入点、表单、隐藏参数和请求

3. `Evidence / readiness check`
   - 是否足以支撑 vulnerability
   - 是否足以支撑 impact
   - 是否足以支撑 novelty / severity 说明

## 工具要求

允许并鼓励使用：
- Chrome DevTools
- Chrome MCP
- 浏览器源码查看
- XHR / fetch 请求观察
- HTML / JS / CSS / bundle 分析
- object ID / token / state / hidden param 检查

默认原则：
- 能进请求层就不要只停在 UI 层
- 能验证参数 / 对象边界就不要只描述页面存在
- 默认优先做低风险、低频、定向验证

## 切换到下一个项目的条件

只有当以下条件都满足时，才自动进入下一个项目：

- program / scope 已确认
- 攻击面已映射
- 登录前 UI review 已完成
- 登录前 DevTools 深查已完成
- 五类基线检查已完成
- 如存在现成登录态，则登录后核心检查也已完成
- 已确认是“有漏洞并保留细节”或“无漏洞并简记入 master-tracker”

## 标签页清理规则

每个项目结束后：
- 关闭该项目专门打开的标签页
- 保留后续项目需要复用的最少页面
- 避免让旧项目的登录态、会话、缓存或标签页污染下一个项目

## 当前推进原则

后续继续按 `partb/target-selection.md` 的目标清单推进。  
默认顺序是：

1. 先做登录前批量检查
2. 发现已有登录态则自动补做登录后检查
3. 无漏洞只简记总表
4. 有漏洞才展开详细记录
