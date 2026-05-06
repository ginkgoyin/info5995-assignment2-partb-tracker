# Part B AI 辅助找漏洞操作文档（中文版）

适用场景：INFO5995 Assignment 2 Part B。目标是合理利用 Codex、网页 GPT、Chrome DevTools / Network 证据来辅助寻找、验证、整理和展示 bug bounty finding。核心原则：AI 负责辅助分析和整理，人类负责合法性、测试边界、最终判断和提交内容真实性。

---

## 0. 总原则

AI 不负责“自动攻击网站”。AI 的合理作用是：整理 scope 和规则证据、分析 DevTools Network 请求/响应、比较 Account A / Account B / anonymous 的访问差异、判断是否存在安全边界失败、草拟 finding summary / novelty analysis / presentation script / Q&A，以及生成本地分析脚本。

人类负责选择合法 bug bounty target、阅读 scope 和 out-of-scope、控制测试强度、只使用自控账号和自控内容、判断是否停止、最终确认所有材料真实、准确、合规。

---

## 1. 推荐工具分工

| 工具 | 用途 | 不建议做什么 |
|---|---|---|
| Chrome DevTools | 手动观察 Network、Console、Storage、Response | 不要批量扫描或爆破 |
| Codex | 本地整理 HAR/JSON/headers，写 diff 脚本，生成表格，清洗敏感信息 | 不要让它自动对真实目标发大量请求 |
| Chrome MCP + Codex | 辅助重复性浏览、截图、下载页面证据 | 不要让它自动点击危险按钮、提交表单、删除资源 |
| 网页 GPT | 分析 scope、rubric、finding summary、novelty、severity、Q&A | 不要上传未打码 cookie/token |
| 人类成员 | 最终判断、控制测试边界、决定是否继续 | 不要让 AI 独立决定攻击范围 |

---

## 2. 文件夹结构

```text
partB-ai-workspace/
  00-assignment/
    assignment2-spec-4.pdf
    assignment2-rubric-3.pdf
    ai-usage-log.md
  01-scope/
    program-page-screenshot.png
    in-scope-assets.md
    out-of-scope-rules.md
    testing-boundaries.md
  02-test-accounts/
    account-model.md
    account-a-notes.md
    account-b-notes.md
    anonymous-context-notes.md
  03-devtools-captures/
    raw/
      account-a.har
      account-b.har
      anonymous.har
    redacted/
      account-a-redacted.har
      account-b-redacted.har
      anonymous-redacted.har
    screenshots/
    console-logs/
    headers/
  04-analysis/
    request-inventory.csv
    response-diff.csv
    suspicious-findings.md
    false-positives.md
  05-candidates/
    candidate-001-instagram-cdn/
      finding-summary.md
      reproduction-steps.md
      impact-analysis.md
      novelty-analysis.md
      severity-mapping.md
      mitigation.md
      qa-prep.md
  06-presentation/
    partB-script.md
    slide-outline.md
    q-and-a.md
```

---

## 3. 安全边界

测试前先写下：

```text
Target:
Program:
In-scope URL:
Out-of-scope restrictions:
Accounts used:
Data used:
Maximum test intensity:
Stop conditions:
```

### 允许做

```text
- 使用自己的测试账号 A 和 B
- 创建无害测试内容，例如 TEST-MARKER-123
- 比较 A / B / anonymous 是否能看到同一资源
- 保存少量 DevTools 请求/响应
- 只复现一两个样本
- 截图、记录 status code、headers、response size
- 分析前端和后端数据交换
```

### 不允许做

```text
- 扫描大量 URL / 子域名
- 暴力猜测 ID、token、用户名、密码
- 访问真实用户数据
- 绕过验证码、登录、2FA
- DoS、压力测试、频繁请求
- 修改或删除不属于自己的资源
- 让 AI 自动执行不受控浏览或提交
- 上传 cookie、Authorization、CSRF token 到 AI
```

如果怀疑漏洞高于 Medium，先和 tutor / unit coordinator 做 sanity-check，不要做破坏性测试。

---

## 4. 每次测试的标准流程

### Step 1：确认 scope

保存官方 bug bounty program 页面截图、in-scope assets 截图、out-of-scope rules 截图、测试目标 URL、日期和时间。

给网页 GPT 的 prompt：

```text
Please review the attached bug bounty scope evidence.
Tell me:
1. Is the target likely in scope?
2. What are the relevant out-of-scope restrictions?
3. What legal/safe testing boundaries should we state in our Part B presentation?
4. What screenshots should we keep as evidence?

Do not invent facts. Only use the uploaded screenshots/text.
```

### Step 2：设计账号模型

```text
Account A: resource owner
Account B: non-owner / non-follower / removed member / blocked user
Anonymous: logged-out browser, no cookies
```

原则：A 应该能访问资源；B 或 anonymous 不应该访问私有资源。如果 B 或 anonymous 能看到不该看的数据，才可能是漏洞。

### Step 3：用 DevTools 记录证据

```text
1. 打开 DevTools
2. Network → 勾选 Preserve log
3. 勾选 Disable cache
4. 清空 Network
5. 执行测试步骤
6. 右键请求列表 → Save all as HAR with content
7. 同时截图页面状态
8. 记录时间戳
```

每组身份单独保存：`account-a.har`、`account-b.har`、`anonymous.har`。

### Step 4：打码和清洗

不要直接把 raw HAR 发给 AI。先让 Codex 在本地清洗。

给 Codex 的 prompt：

```text
You are working only on local evidence files for an authorized university bug bounty assignment.

Task:
1. Read HAR files from ./03-devtools-captures/raw/
2. Create redacted copies in ./03-devtools-captures/redacted/
3. Remove or replace sensitive values:
   - Cookie
   - Authorization
   - CSRF tokens
   - session IDs
   - access tokens
   - email addresses
   - phone numbers
   - personal names
4. Preserve:
   - request URL path
   - method
   - status code
   - content-type
   - response size
   - non-sensitive parameter names
   - timing
5. Produce request-inventory.csv with:
   - source_file
   - method
   - url_origin
   - url_path
   - query_parameter_names
   - status
   - content_type
   - response_size
   - notes
6. Do not send any network requests. Only process local files.
```

Codex 输出后，人类检查 redacted 文件，确认没有 token/cookie 再交给网页 GPT 分析。

---

## 5. AI 分析请求/响应的标准模板

把 redacted HAR、截图、步骤说明发给网页 GPT，然后用这个 prompt：

```text
I am working on an authorized university bug bounty assignment.
All testing used self-controlled accounts and in-scope targets.

Please analyze the uploaded redacted DevTools evidence.

Context:
- Account A is the resource owner.
- Account B is not authorized to access Account A's private resource.
- Anonymous means logged-out browser with no cookies.

Please answer:
1. What is the expected access-control behaviour?
2. What is the observed behaviour for Account A, Account B, and anonymous?
3. Is there any evidence of unauthorized read access, write access, metadata leakage, or privacy-boundary mismatch?
4. Which requests/responses are most important?
5. What additional evidence is needed before calling this a vulnerability?
6. Is this likely: no issue, product bug only, Low, Medium, High/Critical?
7. What should we avoid claiming?
8. Draft a conservative finding title and summary.

Do not exaggerate the impact. Do not assume facts not present in the evidence.
```

---

## 6. 优先测试的漏洞类型

### 6.1 权限越权 / IDOR

重点参数：`post_id`、`media_id`、`comment_id`、`user_id`、`owner_id`、`profile_id`、`account_id`、`resource_id`。

测试模式：A 创建资源 X；B 不应该访问 X；B 打开 X 的 URL 或触发相同 API；观察是否返回 X 的内容、metadata、media URL、owner 信息。

有效迹象：B 能看到 A 的私有内容、anonymous 能看到私有内容、B 能修改/删除 A 的资源、response 返回前端没展示但敏感的数据。

无效迹象：只是 404/403、只是 console error、public 内容被 public 用户看到、没有安全影响的 UI bug。

### 6.2 状态变化后的权限不一致

重点状态：

```text
public → private
shared → unshared
member → removed
allowed → blocked
link enabled → link revoked
post visible → deleted
```

标准测试：

```text
1. A 创建 public/private 测试资源
2. 保存页面 URL 和相关 media/API URL
3. 改变权限或状态
4. B 和 anonymous 再访问页面 URL
5. B 和 anonymous 再访问直接资源 URL
6. 记录 status code、response size、截图、时间戳
7. 延迟复测一次
```

### 6.3 后端响应泄露前端不显示的数据

在 Network response 里搜索：`email`、`phone`、`token`、`secret`、`owner`、`viewer`、`privacy`、`is_private`、`media`、`cdn`、`caption`、`location`、`role`、`permission`。

比较：A response vs B response；A response vs anonymous response；public state response vs private state response；before revoke vs after revoke。

---

## 7. 判断是否符合 Part B 的决策树

```text
Q1: Target 是否在合法 bug bounty scope 内？
否 → 不能用。
是 → Q2。

Q2: 是否只用了自控账号/自控内容，并遵守 out-of-scope？
否 → 停止，不要用。
是 → Q3。

Q3: 是否有可复现的异常行为？
否 → 不能作为 finding。
是 → Q4。

Q4: 异常是否跨越安全边界？
例如 unauthorized read/write、privacy bypass、data leakage。
否 → 可能只是普通 bug。
是 → Q5。

Q5: 是否有清晰 impact？
谁是攻击者？谁是受害者？什么数据/操作被影响？现实后果是什么？
否 → impact evidence 弱。
是 → Q6。

Q6: 是否查过公开披露？
否 → 不能 claim zero-day。
是 → Q7。

Q7: 是否找到完全相同公开报告？
是，并且现在仍可复现 → Type 1 confirmed non-zero-day。
是，但已经修复 → 不计。
否 → Type 2 zero-day candidate。
如果 vendor 后来确认是未公开重复 → zero-day duplicate。
```

---

## 8. Severity 初步判断

| 情况 | 建议 severity |
|---|---|
| 只有 console error，没有安全影响 | S0 |
| 已知 direct URL 仍可访问，但需要攻击者提前拿到 URL | S1，可能 S2 |
| B 能读 A 的私有 metadata | S1–S2 |
| B 能读 A 的私有内容 | S2，影响大时可能 S3 |
| B 能修改/删除 A 的资源 | S2–S3 |
| 普通用户能越权管理员操作 | S3 |
| 大规模账号接管、敏感数据批量泄露 | S4，但需要非常强证据 |

---

## 9. Novelty 搜索流程

每个 candidate 都建一个 `novelty-analysis.md`：

```text
Candidate title:
Tested target/version/date:
Search date:
Search engine:
Search terms:
Sources checked:
Similar reports found:
Why similar:
Why different:
Conclusion:
Finding status:
```

搜索关键词模板：

```text
[platform] [feature] unauthorized access
[platform] [feature] IDOR
[platform] private media CDN URL
[platform] stale direct URL after privacy change
[platform] revoked link still accessible
[platform] bug bounty [vulnerability class]
[platform] HackerOne disclosed [vulnerability class]
[platform] CVE [feature]
```

让网页 GPT 分析 novelty：

```text
Please analyze the uploaded novelty evidence.

Candidate:
[one-paragraph summary]

Public sources checked:
[list]

Similar reports:
[list]

Please decide:
1. Is there a public disclosure that appears to match the exact same vulnerability?
2. Are the similar reports only related prior art, or equivalent?
3. Should we classify this as Type 1, Type 2, zero-day duplicate, or unsupported novelty?
4. What wording should we use in the presentation to avoid overclaiming?
```

---

## 10. Candidate 记录模板

```text
# Candidate ID:
# Date:
# Tester:
# Program:
# Target URL:
# In-scope evidence:
# Accounts used:

## Expected behaviour

## Observed behaviour

## Reproduction steps

1.
2.
3.

## Key evidence

- Screenshot:
- HAR:
- Request:
- Response:
- Status code:
- Content-Type:
- Response size:
- Timestamp:

## Security boundary

Attacker:
Victim:
Asset:
Boundary crossed:

## Impact

## Severity estimate

## Novelty evidence

## Finding status

Type 1 / Type 2 / zero-day duplicate / invalid

## Mitigation

## Open questions

## Decision

Use / keep as backup / reject
```

---

## 11. AI usage log 模板

```text
# AI Usage Log

## Entry 1
Date:
Tool:
Input summary:
Output summary:
How we used it:
Human validation:
Changes made:

## Entry 2
Date:
Tool:
Input summary:
Output summary:
How we used it:
Human validation:
Changes made:

## Mock Q&A
Date:
Rubric version:
Questions asked:
Weak answers identified:
Evidence added:
Presentation changes:
```

---

## 12. 给 Codex 的本地分析任务模板

### 12.1 生成 HAR 清洗脚本

```text
Write a Python script that redacts HAR files for safe analysis.

Requirements:
- Input folder: ./03-devtools-captures/raw/
- Output folder: ./03-devtools-captures/redacted/
- Remove Cookie, Authorization, X-CSRF, session IDs, access tokens
- Replace email-like strings with [REDACTED_EMAIL]
- Replace phone-like strings with [REDACTED_PHONE]
- Keep method, host, path, status, content-type, response size
- Generate request-inventory.csv
- Do not send network requests
- Do not install external packages unless necessary
```

### 12.2 比较 A / B / anonymous

```text
Analyze the redacted HAR files locally.

Goal:
Compare Account A, Account B, and anonymous requests.

Output:
1. endpoints-common.csv
2. endpoints-only-account-a.csv
3. endpoints-only-account-b.csv
4. endpoints-only-anonymous.csv
5. suspicious-differences.md

Flag suspicious cases where:
- Account B or anonymous receives 200 for a resource that should be private
- response size is similar to Account A for private content
- response contains media/cdn/url/caption/owner fields
- status differs from expected 403/404/private barrier
```

### 12.3 整理 presentation evidence

```text
Using the markdown notes in ./05-candidates/, create:
1. a 5-minute presentation outline
2. a speaker allocation plan for 5 team members
3. a Q&A preparation file
4. a one-page evidence checklist

Constraints:
- Do not invent evidence.
- Mark missing evidence clearly.
- Use conservative severity.
- Include finding status under the updated 4 May rules.
```

---

## 13. 给网页 GPT 的判断模板

```text
I will provide a candidate vulnerability summary and redacted evidence.

Please evaluate it under the INFO5995 Assignment 2 Part B rubric.

Focus on:
1. Scope handling
2. Reproducibility
3. Security boundary
4. Impact evidence
5. Severity tier S0-S4
6. Finding status under the updated 4 May rules:
   - Type 1 confirmed non-zero-day
   - Type 2 zero-day candidate
   - zero-day duplicate if applicable
7. Novelty evidence gaps
8. Whether this should be our final Part B finding

Be conservative. Point out weaknesses. Do not help perform unsafe testing.
```

---

## 14. 最终 presentation 结构

```text
0:00–0:40  Scope and legal testing boundary
0:40–1:30  Feature and expected security model
1:30–2:30  Reproduction steps
2:30–3:20  Evidence: request/response/screenshots
3:20–4:00  Impact and severity mapping
4:00–4:35  Novelty and finding status
4:35–5:00  Mitigation and conclusion
```

每位组员至少说 40 秒；Part B presentation 保持在 5 分钟以内。

---

## 15. 对当前 Instagram candidate 的 AI 工作流

```text
1. 人类复现：
   - A public post
   - capture direct CDN URL
   - A switches private
   - B/anonymous blocked on post page
   - anonymous still gets 200 on direct CDN URL

2. Codex：
   - 清洗 HAR
   - 输出 request inventory
   - 比较 post page request 和 CDN request
   - 提取 status/content-type/content-length/time

3. 网页 GPT：
   - 判断是否 privacy-boundary mismatch
   - 判断 impact 是否足够
   - 分析 novelty：是否已有完全相同公开披露

4. 人类：
   - 决定是否继续用作 final finding
   - 不夸大 severity
   - 不访问第三方内容
```

推荐展示口径：

```text
Finding status:
Type 2 zero-day candidate with related prior-art caveat, unless tutors consider related Instagram private-media reports equivalent.

Severity:
Conservatively S1 Low; potentially S2 Medium if the expected privacy model requires media URLs to be revoked or protected after public-to-private transition.

Impact:
A user who switches an Instagram account from public to private may still have previously exposed media retrievable by anyone who already obtained the direct CDN URL.
```
