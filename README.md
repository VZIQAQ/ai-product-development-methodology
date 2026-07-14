# AI Product Development Methodology: From Idea to Working Product
# AI产品开发方法论：从想法到可运行产品

> **核心理念 / Core Principle**: AI is a super-executor, not a product owner. You think, judge, and validate; AI writes, draws, and builds.
>
> **核心观点**：AI是超级执行者，不是产品负责人。你负责想、判断、验证，AI负责写、画、编。

---

## Table of Contents / 目录

- [Why AI Always Gets Your Idea Wrong / 为什么AI总是做错](#why-ai-always-gets-your-idea-wrong--为什么ai总是做错)
- [Information Granularity: Where to Be Detailed, Where to Be Vague / 信息粒度：哪里详细，哪里模糊](#information-granularity--信息粒度)
- [Pre-Development: Spec Extraction and Locking / 开发前准备：Spec提取与锁定](#pre-development-spec--开发前准备spec)
- [Test-First: Confirming "What Counts as Right" Before Writing Code / 测试前置：写代码前确认"什么算对"](#test-first--测试前置)
- [Strict Serial Iteration: No Changes During Development / 严格串行迭代：开发中不接受变更](#strict-serial-iteration--严格串行迭代)
- [Delivery and Handover: Discover, Record, Don't Cross Boundaries / 交付与交接：发现问题，记录问题，不跨边界](#delivery-and-handover--交付与交接)
- [Flow Description: When to Be Detailed / 流程描述：什么时候详细](#flow-description--流程描述)
- [What You Need at Each Stage / 各阶段你需要做什么](#what-you-need-at-each-stage--各阶段能力要求)
- [Core Principles Summary / 核心原则总结](#core-principles-summary--核心原则总结)
- [Appendix: Optional Cost Transparency / 附录：可选成本透明](#appendix-cost-transparency--附录成本透明)

---

## Why AI Always Gets Your Idea Wrong / 为什么AI总是做错

You tell AI "build an e-commerce mini app," and it generates:

你给AI说"做个电商小程序"，它生成了：

- Users can place orders ✓ / 用户能下单 ✓
- But can't pay (no payment exception handling) / 但不能支付（没处理支付异常）
- Inventory oversold (no concurrency control) / 库存超卖（没写并发控制）
- Anyone can see everyone's orders (no permission rules) / 任何人能看到所有人订单（没写权限）

**The problem isn't AI. It's the information you gave AI.**

**问题不在AI，在你给AI的信息。**

AI is not human. It cannot "read between the lines." What seems "obvious" in your head—exception branches, boundary conditions, permission rules—if not explicitly stated, AI will implement in the "most generic way," often not matching your real intent.

AI不是人类，它不会"意会"。你脑子里"显然"的东西——异常分支、边界条件、权限规则——如果不明确说出来，AI就会按"最通用的方式"实现，结果往往不符合你的真实意图。

### Implicit Knowledge Is the Biggest Trap / 隐性知识是最大的坑

| What You Think AI Knows / 你以为AI知道的 | What AI Actually Does / 实际AI会怎么做 |
|---|---|
| "After payment, order enters processing" / "订单付款后进入处理状态" | Only handles successful payment, ignores failure, timeout, duplicate callbacks / 只处理付款成功，不处理失败、超时、重复回调 |
| "Admin can see all data" / "管理员能看到所有数据" | No permission checks, all APIs public / 不做权限校验，所有接口公开 |
| "If error, prompt user" / "如果出错就提示用户" | Returns generic "system error," no scene differentiation / 统一返回"系统错误"，不区分场景 |

**Conclusion**: AI is a super-executor, not a product owner. It faithfully executes the information you provide; it will not fill in what you left unsaid.

**结论**：AI是超级执行者，不是产品负责人。它忠实地执行你给的信息，不会帮你补全没说的。

---

## Information Granularity: Where to Be Detailed, Where to Be Vague / 信息粒度：哪里详细，哪里模糊

### Parts That Must Be Described in Detail (Zero Ambiguity) / 必须详细描述的部分（零歧义）

If these are described vaguely, AI will implement in the most generic way, often not matching your real intent.

这些如果描述模糊，AI会按最通用的方式实现，结果往往不符合你的真实意图。

**You don't need to write code. You only need to describe business logic clearly in natural language. The Agent will translate it into precise technical definitions and show key decision explanations for your confirmation.**

**你不需要写代码，只需要用自然语言描述清楚业务逻辑。Agent会推导技术实现，并展示关键决策说明供你确认。**

#### Business Rules and State Transitions / 业务规则与状态流转

❌ Vague / 模糊：
> "After order payment, enters processing state"
> "订单付款后进入处理状态"

✅ Detailed / 详细：
> "After user places order, status is 'pending payment.' After successful payment, becomes 'paid,' system automatically deducts inventory, no manual action needed. After merchant uploads tracking number, becomes 'shipped.' User confirms receipt, or auto-confirms 7 days after shipping, becomes 'completed.' If user applies for refund and order status is 'paid' or 'shipped,' becomes 'refunding.'"
>
> "用户下单后是'待付款'。付款成功后变'已付款'，系统自动扣库存，不需要人工操作。商家上传物流单号后变'已发货'。用户确认收货，或者发货7天后自动确认，变'已完成'。用户申请退款且订单是'已付款'或'已发货'，变'退款中'。"

**Agent will derive and show**: State definitions, trigger conditions, execution actions, error codes, concurrency control strategies.

**Agent会推导并展示**：状态定义、触发条件、执行动作、错误码、并发控制策略。

#### Data Entities and Relationships / 数据实体与关系

❌ Vague / 模糊：
> "User has orders, orders have products"
> "用户有订单，订单有商品"

✅ Detailed / 详细：
> "Each user has one phone number, cannot be duplicated. One order can contain multiple products. Each product in the order must record the purchase price at that time, because product prices may change later, but order prices cannot change. Order total amount uses cents not yuan, to avoid decimals."
>
> "每个用户有一个手机号，不能重复。一个订单可以包含多个商品。每个商品在订单里要记录当时的购买价格，因为以后商品价格可能变，但订单里的价格不能变。订单总金额用分不用元，避免小数。"

**Agent will derive and show**: Table structures, field types, foreign key relationships, constraint conditions.

**Agent会推导并展示**：表结构、字段类型、外键关系、约束条件。

#### Permissions and Visibility / 权限与可见性

❌ Vague / 模糊：
> "Admin can see all data"
> "管理员能看到所有数据"

✅ Detailed / 详细：
> "Super admin can see and modify everything. Regular users can only see their own orders and personal info. Unlogged users can only see product list and details, and only products currently on sale. Sensitive info like phone numbers and ID numbers must never return to frontend under any circumstances, unless user is viewing their own profile."
>
> "超级管理员什么都能看都能改。普通用户只能看自己的订单和个人信息。没登录的人只能看正在卖的商品。手机号和身份证号这种敏感信息，不管什么情况都不能返回给前端，除非用户自己在看自己的资料。"

**Agent will derive and show**: Role permission matrix, data filtering rules, sensitive field masking strategies.

**Agent会推导并展示**：角色权限矩阵、数据过滤规则、敏感字段脱敏策略。

#### Exceptions and Boundaries / 异常与边界

❌ Vague / 模糊：
> "If error, prompt user"
> "如果出错就提示用户"

✅ Detailed / 详细：
> "If inventory insufficient, tell user 'Only X items left.' If two people buy last item simultaneously, only one succeeds, prompt 'Too slow, item sold out.' If WeChat payment callback repeats, must not double-charge. If third-party API times out, retry 3 times, 2 seconds each, still failing mark as 'processing,' manual intervention needed."
>
> "库存不够时告诉用户'还剩X件'。两个人同时买最后一件，只能一个人买到，提示'手慢了'。微信支付重复回调不能重复扣款。第三方API超时重试3次，每次2秒，还不行就标记'处理中'让人工介入。"

**Agent will derive and show**: Error codes, prompt copy, concurrency control, idempotency handling, retry strategies.

**Agent会推导并展示**：错误码、提示文案、并发控制、幂等处理、重试策略。

### Parts That Can Be Vague (Let AI Handle) / 可以模糊的部分（让AI发挥）

| Domain / 领域 | Example / 示例 | Why It Can Be Vague / 为什么可以模糊 |
|---|---|---|
| Visual Style / 视觉风格 | "Tech feel, dark background, clean like Notion" / "科技感，深色背景，类似Notion的简洁感" | AI can generate UI matching description / AI能生成符合描述的UI |
| Standard Features / 标准功能 | "Need user login and registration" / "需要用户登录注册" | Generic pattern, AI auto-implements standard solution / 通用模式，AI自动实现标准方案 |
| Generic Interactions / 通用交互 | "Form should have loading state after submit" / "表单提交后要有加载状态" | AI does this by default / AI默认就会做 |
| Tech Stack Preference / 技术栈偏好 | "Use React + Node.js" / "用React+Node.js" | Only specify direction, AI picks stable details / 只需指定方向，细节AI选稳定的 |
| Responsive Design / 响应式设计 | "Should work on mobile and desktop" / "要适配手机和电脑" | Standard requirement, AI auto-handles / 标准需求，AI自动处理 |

**Principle**: Describe core competitive advantages in detail; keep generic solutions vague.

**原则**：核心竞争力详细描述，通用方案模糊带过。

---

## Pre-Development: Spec Extraction and Locking / 开发前准备：Spec提取与锁定

### What Is Spec / Spec是什么

Spec (Specification) is the **final locked document** before development. It is the "contract" between user and Agent.

Spec（规范说明书）是开发前的**最终锁定文档**，是用户与Agent之间的"合同"。

It is automatically extracted, organized, and confirmed from prior conversations, clearly defining:

它从前期对话中**自动提取、整理、确认**，明确：

- What the product does (scope) / 产品要做什么（功能范围）
- What the product does NOT do (boundaries) / 产品不做什么（边界）
- What counts as correct (acceptance criteria) / 怎么做是对的（验收标准）

### Where Spec Comes From / Spec从哪来

```
Prior Conversation (Vision Exploration + MVP Convergence)
前期对话（愿景探索 + MVP收敛）
    ↓
Agent Auto-Organizes: Feature list, state machine, data model, permission rules, exception handling
Agent自动整理：功能清单、状态机、数据模型、权限规则、异常处理
    ↓
Agent Proactively Prompts Uncovered Scenarios: "The following scenarios are not covered, please consider: ____"
Agent主动提示未覆盖场景："以下场景未覆盖，建议考虑：____"
    ↓
User Confirms: Does this Spec match my real needs?
用户确认：这个Spec是否符合我的真实需求？
    ↓
Lock Spec → Enter Development
锁定Spec → 进入开发
```

**Example of Agent Proactive Prompt / Agent主动提示示例**：

> "Based on current Spec, the following scenarios are not covered, please confirm:
> - After user cancels order, is inventory released immediately?
> - If merchant rejects refund, can user apply again?
> - How long after order completion can user leave review? Is review entry closed after timeout?
>
> 基于当前Spec，以下场景未覆盖，请确认：
> - 用户取消订单后，库存是否立即释放？
> - 商家拒绝退款时，用户能否再次申请？
> - 订单完成后多久内可评价？超时是否关闭评价入口？"

### Locking Principles / 锁定原则

Once locked, Spec becomes a **baseline that cannot be arbitrarily changed**:

一旦锁定，Spec成为**不可随意变更的基准**：

- Agent must strictly generate code according to Spec / Agent必须严格按照Spec生成代码
- Any change must first modify Spec, then regenerate code / 任何变更必须先修改Spec，再重新生成代码
- Spec version and code version correspond one-to-one / Spec版本与代码版本一一对应

**Purpose**: Prevent "requirement drift"—constant changes during development leading to inability to complete.

**目的**：防止"需求漂移"——开发过程中不断变更导致无法完成。

---

## Test-First: Confirming "What Counts as Right" Before Writing Code / 测试前置：写代码前确认"什么算对"

### Test Cases Generated by Agent / 测试用例由Agent生成

Agent automatically generates test cases **after Spec is locked but before code is written**, based on Spec:

Agent在**Spec锁定后、代码生成前**，基于Spec自动生成测试用例：

| Test Type / 测试类型 | Content / 内容 |
|---|---|
| Normal Flow Test / 正常流程测试 | Main flow is smooth / 主流程是否通畅 |
| Exception Flow Test / 异常流程测试 | Boundary conditions, error handling / 边界条件、错误处理 |
| Business Rule Validation / 业务规则验证 | Permissions, state transitions, data constraints / 权限、状态流转、数据约束 |

### User Judges If It Matches Needs / 用户判断是否符合需求

After Agent generates test cases, **user judges each one**:

Agent生成测试用例后，**用户逐一判断**：

```
[Test-001] Normal Order Flow / 正常下单流程
  Steps: Login → Browse → Add to Cart → Place Order → Pay → Receive
  步骤：登录 → 浏览 → 加购 → 下单 → 支付 → 收货
  Expected: State transitions correct, inventory deducted correctly
  预期：状态流转正确，库存扣减正确
  → Does this match your expectation? [Yes] [No, because: ____]
  → 是否符合预期？ [符合] [不符合，因为：____]

[Test-002] Insufficient Inventory Exception / 库存不足异常
  Steps: Place order when inventory is 0
  步骤：下单时库存为0
  Expected: Prompt "insufficient inventory," order not created
  预期：提示"库存不足"，订单不生成
  → Does this match your expectation? [Yes] [No, because: ____]
  → 是否符合预期？ [符合] [不符合，因为：____]
```

**User only needs to judge "does this test cover what I care about," no need to write test code.**

**用户只需判断"这个测试是否测到了我关心的点"，不需要写测试代码。**

### Test Cases as Acceptance Criteria / 测试用例作为验收标准

- Agent writes code targeting "make tests pass" / Agent写代码时，以"让测试通过"为目标
- After development, Agent runs tests, user sees "pass/fail" / 开发完成后，Agent运行测试，用户看"通过/失败"
- Tests pass = product matches Spec = deliverable / 测试通过 = 产品符合Spec = 可以交付

---

## Strict Serial Iteration: No Changes During Development / 严格串行迭代：开发中不接受变更

### Core Principle / 核心原则

> **After Spec is locked, development runs to completion in one go. No requirement changes are accepted during development.**
>
> **Spec锁定后，开发一次性跑完。开发过程中不接受任何需求变更。**

AI is fast enough that we don't need human development's "flexibility to insert requirements mid-stream." Each round is complete: lock → execute → deliver.

AI做得够快，所以不需要人类开发那种"中途插需求"的灵活性。每一轮都是完整的：锁定 → 执行 → 交付。

```
Round 1: Requirements → Spec Lock → Development → Delivery → Acceptance
Round 1: 需求 → Spec锁定 → 开发 → 交付 → 验收
    ↓
Round 2: New Requirements → Modify Spec → Re-lock → Development → Delivery
Round 2: 新需求 → 改Spec → 重新锁定 → 开发 → 交付
    ↓
Round 3: ...
```

### Strict Boundaries During Development / 开发阶段严格边界

**Development phase only fixes "what was written wrong," not "what wasn't written."**

**开发阶段只修"写错了的"，不修"没写的"。**

| Situation / 情况 | Attribution / 归属 | Handling / 处理方式 | Stage / 阶段 |
|---|---|---|---|
| Acceptance test fails (feature in Spec, but implementation wrong) / 验收测试失败（功能在Spec里，但实现不对） | Execution deviation / 执行偏差 | Fix within Round 1 / Round 1内修复 | Development / 开发阶段 |
| Acceptance discovers missing scenario (feature should be in Spec, but forgotten) / 验收发现遗漏场景（功能该在Spec里，但忘了写） | Spec incomplete / Spec不完善 | **Rollback to Spec phase**, complete and re-lock / **回退到Spec阶段**，补全后重新锁定 | Cannot fix in development / 不能开发阶段修 |
| Urgent bug not in Spec / 紧急Bug但不在Spec里 | Spec incomplete / Spec不完善 | Log in requirement pool, next round iteration / 记入需求池，下一轮迭代 | Cannot fix in development / 不能开发阶段修 |
| New requirement / 新需求 | New requirement / 新需求 | Log in requirement pool, next round iteration / 记入需求池，下一轮迭代 | Cannot fix in development / 不能开发阶段修 |

### Prohibited Fix Handling Process / 禁止修复的处理流程

```
Discover "not in Spec" issue / 发现"不在Spec里"的问题
    ↓
Mark as "Spec omission" / 标记为"Spec遗漏"
    ↓
Log in requirement pool / 记入需求池
    ↓
Wait for next round iteration: Complete Spec → Re-lock → Develop
等待下一轮迭代：补Spec → 重新锁定 → 开发
```

### Responsibility Attribution / 责任归属

- **Execution deviation** → Development responsibility, fix in development phase / **执行偏差** → 开发责任，开发阶段修
- **Spec omission** → User/requirement responsibility, rollback to Spec phase / **Spec遗漏** → 用户/需求责任，回退到Spec阶段
- **New requirement** → User responsibility, queue for next round / **新需求** → 用户责任，排队到下一轮

> **Development phase is "executing the contract," not "renegotiating the contract."**
>
> **开发阶段是"执行合同"，不是"重新谈判"。**
>
> Contract has loopholes, renegotiation happens another time. Changes are not forbidden; they queue for the next round.
>
> 合同有漏洞，谈判另找时间。变化不是禁止，是排队到下一轮。

---

## Delivery and Handover: Discover, Record, Don't Cross Boundaries / 交付与交接：发现问题，记录问题，不跨边界

### Delivery Contents / 交付内容

After development completes, Agent delivers:

开发完成后，Agent交付：

- Working product / 可运行的产品
- Test report (what passed, what failed) / 测试报告（哪些通过、哪些失败）
- **Handover document** / **交接文档**

### Handover Document Contents / 交接文档内容

Handover document records all **issues discovered during development that were not covered in Spec**:

交接文档记录本轮开发中发现的所有**未在Spec中覆盖的问题**：

```markdown
# Round 1 Handover Document / Round 1 交接文档

## Delivered Features / 已交付功能
(List each item from Spec with test pass status)
(按Spec逐项列出，标注测试通过状态)

## Issues Discovered During Development (Not in Spec) / 开发过程中发现的问题（未在Spec中）

### Issue-001: Inventory Release Timing After User Cancellation / 问题-001：用户取消订单后库存释放时机
- Discovery timing: When developing order cancellation feature / 发现时机：开发订单取消功能时
- Current Spec status: Not defined / 当前Spec状态：未定义
- Impact: May cause inventory data inconsistency / 影响：可能导致库存数据不一致
- Suggestion: Supplement Spec in Round 2, clarify whether inventory releases immediately after cancellation / 建议：Round 2补充Spec，明确取消后库存是否立即释放

### Issue-002: User Re-application After Merchant Rejects Refund / 问题-002：商家拒绝退款后的用户再次申请
- Discovery timing: When developing refund flow / 发现时机：开发退款流程时
- Current Spec status: Not defined / 当前Spec状态：未定义
- Impact: User may apply for refund unlimited times / 影响：用户可能无限次申请退款
- Suggestion: Supplement Spec in Round 2, clarify whether re-application is allowed after rejection and limit count / 建议：Round 2补充Spec，明确拒绝后是否允许再次申请及次数限制

## Requirement Pool (Pending Round 2 and beyond) / 需求池（待Round 2及以后处理）
- [ ] Issue-001: Order cancellation inventory release / 问题-001：取消订单库存释放
- [ ] Issue-002: Refund re-application limit / 问题-002：退款再次申请限制
- [ ] User suggestion: Add coupon feature / 用户提议：增加优惠券功能
- [ ] User suggestion: Add order export feature / 用户提议：增加订单导出功能
```

### Core Principle / 核心原则

> **Who's problem is fixed where they are; boundaries are not crossed.**
>
> **谁的问题在谁那里改，不能跨越。**
>
> Development discovers Spec loophole → Log issue → Hand to user/requirement side → User decides whether to supplement in next round.
>
> 开发发现Spec有漏洞 → 记录问题 → 交给用户/需求方 → 用户决定下一轮是否补充。
>
> Development does not arbitrarily supplement Spec; user does not interfere with development execution.
>
> 开发不擅自补Spec，用户不干涉开发执行。
>
> Both sides interact through handover document to improve product, but **strictly respect boundaries**.
>
> 两边通过交接文档交互，完善产品，但**严格遵守边界**。

---

## Flow Description: When to Be Detailed / 流程描述：什么时候详细

### Judgment Standard for Detailed Description / 需要详细描述的判断标准

**Does this flow have multiple branches or states?**

**这个流程有没有多个分支或状态？**

| Flow Type / 流程类型 | Example / 示例 | Need Detail? / 需要详细吗？ |
|---|---|---|
| Linear Flow / 线性流程 | Fill form → Submit → Success / 填表 → 提交 → 成功 | ❌ No / 不需要 |
| Branch Flow / 分支流程 | Pay success→ship, fail→retry/cancel / 支付成功→发货，失败→重试/取消 | ✅ Yes / 需要 |
| State Transition / 状态流转 | Pending→Paid→Shipped→Completed / 待付款→已付款→已发货→已完成 | ✅ Yes / 需要 |
| Multi-role Collaboration / 多角色协作 | User applies→Merchant reviews→Platform arbitrates / 用户申请→商家审核→平台仲裁 | ✅ Yes / 需要 |
| Loop/Backtrack / 循环/回退 | Review fails→Return for modification→Resubmit / 审核不通过→退回修改→重提 | ✅ Yes / 需要 |

### Lightweight Expression Methods / 轻量表达方式

#### User Story + Acceptance Criteria / 用户故事 + 验收标准

```
As [user role], I want [to do something], so that [achieve goal]
作为[用户角色]，我想[做什么]，以便[达成什么目的]

Acceptance Criteria / 验收标准：
- Given [condition A], when [action], then [result X]
- 给定[条件A]，当[操作]，那么[结果X]
- Given [exception condition], when [action], then [error prompt Z]
- 给定[异常条件]，当[操作]，那么[错误提示Z]
```

#### State Machine Table / 状态机表格

| Current State / 当前状态 | Trigger Condition / 触发条件 | Next State / 下一状态 | Execution Action / 执行动作 |
|---|---|---|---|
| Pending Payment / 待付款 | Payment success / 支付成功 | Paid / 已付款 | Deduct inventory / 扣减库存 |
| Pending Payment / 待付款 | Timeout 30min / 超时30分钟 | Cancelled / 已取消 | Release inventory / 释放库存 |
| Shipped / 已发货 | User confirm/timeout 7 days / 用户确认/超时7天 | Completed / 已完成 | Settle merchant / 结算商家 |

#### Rule List / 规则列表

```
If user role is "regular user" and accessed resource is not their own:
    Deny access
如果用户角色是"普通用户"且访问的不是自己的资源：
    拒绝访问

If order status is "shipped" and over 7 days since shipping:
    Auto-confirm receipt
如果订单状态是"已发货"且超过7天：
    自动确认收货
```

---

## What You Need at Each Stage / 各阶段你需要做什么

| Stage / 阶段 | Your Capability / 你需要的能力 | Agent Will Help / Agent会帮你做 |
|---|---|---|
| Requirement Exploration / 需求挖掘 | Express ideas, answer clarification questions, judge if understanding is accurate / 表达想法、回答澄清问题、判断理解是否准确 | Structure requirements, identify omissions, propose boundary cases / 结构化需求、识别遗漏、提出边界情况 |
| Solution Design / 方案设计 | Confirm business rules, make trade-off decisions, identify industry-specific pitfalls / 确认业务规则、做取舍决策、识别行业毒点 | Decompose features, design data models, recommend technical solutions / 拆解功能、设计数据模型、推荐技术方案 |
| Spec Locking / Spec锁定 | Review Spec, judge if test cases cover concerns / 审阅Spec、判断测试用例是否覆盖关心场景 | Auto-extract and organize Spec, generate test cases / 自动提取整理Spec、生成测试用例 |
| Code Generation / 代码生成 | Test acceptance, describe issues, confirm modification direction / 测试验收、描述问题、确认修改方向 | Generate code per Spec, handle technical details, deploy / 按Spec生成代码、处理技术细节、部署上线 |
| Delivery Acceptance / 交付验收 | Accept product, review handover document, decide next round priorities / 验收产品、审阅交接文档、决定下一轮优先级 | Generate handover document, record uncovered issues, update requirement pool / 生成交接文档、记录未覆盖问题、更新需求池 |
| Continuous Iteration / 持续迭代 | Judge priorities based on feedback, make business decisions / 基于反馈判断优先级、做业务决策 | Analyze change impact, generate solutions, execute modifications / 分析变更影响、生成方案、执行修改 |

---

## Core Principles Summary / 核心原则总结

> **For AI, "detailed" doesn't mean more words; it means less ambiguity.**
>
> **对AI来说，"详细"不是字数多，而是"歧义少"。**

> **Don't give AI "countless flowcharts"; give AI "precise rules for key flows."**
>
> **不要给AI"无数个流程图"，要给AI"关键流程的精确规则"。**
> Linear flows use text, branch flows use tables, state transitions use state machines, complex judgments use rule lists.
> 线性流程用文字，分支流程用表格，状态流转用状态机，复杂判断用规则列表。

> **You don't need to write code; you only need to describe business logic clearly in natural language.**
>
> **你不需要写代码，只需要用自然语言描述清楚业务逻辑。**
> Agent derives technical implementation and shows key decision explanations for your confirmation. Your energy focuses on "is the business correct."
> Agent会推导技术实现，并展示关键决策说明供你确认。你的精力集中在"业务是否正确"。

> **Spec is the contract, tests are the standard; lock before developing.**
>
> **Spec是合同，测试是标准，锁定后再开发。**
> Early full communication, confirmation, and locking; Agent autonomously executes during development, reducing wasted back-and-forth. Accuracy and efficiency are the greatest cost savings.
> 前期充分沟通、确认、锁定，开发阶段Agent自主执行，减少来回修改的浪费。准确高效就是最大的成本节省。

> **Development phase is "executing the contract," not "renegotiating the contract."**
>
> **开发阶段是"执行合同"，不是"重新谈判"。**
> Contract has loopholes, renegotiation happens another time. Changes are not forbidden; they queue for the next round.
> 合同有漏洞，谈判另找时间。变化不是禁止，是排队到下一轮。

> **Who's problem is fixed where they are; boundaries are not crossed.**
>
> **谁的问题在谁那里改，不能跨越。**
> Development discovers Spec loophole → Log → Hand to requirement side → Requirement side decides whether to supplement in next round. Both sides interact through handover document, strictly respecting boundaries.
> 开发发现Spec漏洞 → 记录 → 交给需求方 → 需求方决定下一轮是否补充。两边通过交接文档交互，严格遵守边界。

> **AI is a super-executor, not a product owner.**
>
> **AI是超级执行者，不是产品负责人。**
> It can drive implementation cost to near zero after "thinking clearly," but "thinking clearly" itself, and post-launch validation and iteration, remain irreplaceably human.
> 它能把"想清楚"之后的实现成本降到趋近于零，但"想清楚"本身，以及上线后的验证和迭代，仍然是人不可替代的。

---

## Appendix: Optional Cost Transparency / 附录：可选成本透明

For scenarios requiring fine-grained cost management, Agent can provide estimates as reference:

对于需要精细成本管理的场景，Agent可提供预估作为参考：

| Task / 任务 | Estimated Tokens / 预估Token | Estimated Time / 预估时间 |
|---|---|---|
| Generate Spec / 生成Spec | 2K | 5s |
| Generate Test Cases / 生成测试用例 | 3K | 8s |
| Generate Code / 生成代码 | 10K | 30s |

This is not a required step, but optional auxiliary information. Core goal is to accurately and efficiently complete the product, reducing waste from repeated modifications.

这不是必需步骤，而是可选辅助信息。核心目标是准确高效地完成产品，减少因反复修改造成的浪费。

---

*This methodology is based on the latest 2026 AI Agent software engineering research and practice, continuously iterating.*

*本文方法论基于2026年AI Agent软件工程最新研究与实践，持续迭代中。*

#AI #ProductDevelopment #PRD #Agent #LowCode #IndieDev #SoftwareEngineering #Methodology
