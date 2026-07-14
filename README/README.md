# AI Product Development Methodology / AI产品开发方法论

> **Core Principle / 核心原则**: AI is a super-executor, not a product owner. You think, judge, and validate; AI writes, draws, and builds.
>
> AI是超级执行者，不是产品负责人。你负责想、判断、验证，AI负责写、画、编。

A methodology for developing software products with AI assistance, based on **Spec-Driven Development**, **Test-First Confirmation**, and **Strict Serial Iteration**.

一个基于**Spec驱动开发**、**测试前置确认**和**严格串行迭代**的AI辅助软件开发方法论。

## Key Features / 核心特性

- **Natural Language Driven / 自然语言驱动**: Describe business logic in plain language; Agent derives technical details
  - 用自然语言描述业务逻辑；Agent推导技术细节
- **Spec as Contract / Spec即合同**: Lock specifications before development; changes queue for next round
  - 开发前锁定规范；变更排队到下一轮
- **Test-First / 测试前置**: Agent generates tests before code; user confirms coverage
  - Agent在代码前生成测试；用户确认覆盖度
- **Strict Boundaries / 严格边界**: Execution deviation → fix in dev; Spec omission → rollback to Spec phase
  - 执行偏差 → 开发中修复；Spec遗漏 → 回退到Spec阶段
- **Tool Agnostic / 工具无关**: No tool recommendations; methodology works with any AI coding assistant
  - 无工具推荐；方法论适用于任何AI编程助手

## Documentation / 文档

- [Full Methodology Document / 完整方法论文档](./AI_Product_Development_Methodology_Bilingual.md) - Complete bilingual guide / 完整双语指南

## Quick Start / 快速开始

1. **Describe your idea / 描述你的想法** in natural language (business rules, state transitions, permissions)
   - 用自然语言（业务规则、状态流转、权限）
2. **Agent extracts Spec / Agent提取Spec** from your description + asks clarifying questions
   - 从你的描述中提取 + 问澄清问题
3. **You confirm the Spec / 你确认Spec** - this is your contract
   - 这是你的合同
4. **Agent generates tests / Agent生成测试** based on Spec; you judge coverage
   - 基于Spec；你判断覆盖度
5. **Agent develops / Agent开发** according to locked Spec and confirmed tests
   - 根据锁定的Spec和确认的测试
6. **You accept delivery / 你接受交付** + receive handover document for next round
   - 接收交接文档用于下一轮

## Contributing / 贡献

See [CONTRIBUTING.md](./CONTRIBUTING.md)

## License / 协议

MIT License - see [LICENSE](./LICENSE)
