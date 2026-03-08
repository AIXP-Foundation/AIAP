<!-- markdownlint-disable MD041 -->
<div align="center">
  <h1>AI 应用协议 (AIAP)</h1>
  <p><b>一个用于结构化、验证和治理 AI 程序的开放协议。</b></p>
  <p>
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg" alt="Apache 2.0" /></a>
    <img src="https://img.shields.io/badge/Protocol-AIAP_V1.0.0-brightgreen.svg" alt="AIAP V1.0.0" />
    <img src="https://img.shields.io/badge/Axiom_0-Human_Sovereignty-orange.svg" alt="Axiom 0" />
    <a href="https://aiap.dev"><img src="https://img.shields.io/badge/docs-aiap.dev-blue.svg" alt="Docs" /></a>
  </p>
  <p>
    中文文档 | <a href="README.md">English</a>
  </p>
</div>

## AIAP 是什么？

AI 应用协议 (AIAP) 是一个开放标准，定义了 AI 程序如何被**结构化**、**验证**和**安全约束**。[A2A](https://github.com/a2aproject/A2A) 解决 Agent 之间如何通信，[MCP](https://modelcontextprotocol.io/) 定义模型如何访问工具，而 AIAP 解决的是另一个挑战：**AI 程序本身应该如何被治理**。

AIAP 确保自主 AI 系统以下列方式运行：

- **确定性结构** — 通过 AISOP 蓝图格式 (`.aisop.json`)
- **可验证质量** — 通过 ThreeDimTest 质量框架（正确性、内在质量、细节合规）
- **分级安全** — 通过信任等级 (T1-T4) 和威胁感知验证 (AT1-AT6)
- **不可变对齐** — 通过 Axiom 0：人类主权与福祉

### 为什么需要 AIAP？

随着 AI Agent 变得更强大和更自主，不受控的幻觉、对齐漂移和安全违规的风险也在增长。传统的"提示工程"试图通过添加更多指令来解决问题 — 但这矛盾地增加了熵和歧义。AIAP 采取相反的方法：

1. **语义编译** — AIAP 验证器像编译器一样工作，在运行前拒绝模糊指令、未处理的错误路由和语义矛盾。
2. **零熵共振** — 通过刚性结构模式收缩可能性空间，最小化可用于自由解释的 token。
3. **L0/L1 隔离** — 严格分离机器可读状态 (L0) 和人类可读叙事 (L1)，防止多 Agent 级联中的交叉污染。
4. **Axiom 0 强制执行** — 在协议层面硬对齐"人类主权与福祉"，不是建议而是不可变约束。

## 核心特性

| 特性 | 说明 |
|------|------|
| **AISOP 蓝图** | `.aisop.json` 文件通过 Mermaid 流程图定义 Agent 行为，作为确定性执行路径 |
| **结构模式** | 七种模式 (A-G)，从单脚本到嵌入式运行时（含工具目录） |
| **质量标准** | 50+ 验证规则，覆盖正确性 (C1-C7)、内在质量 (I1-I13)、细节 (D1-D10) 三个维度 |
| **信任等级** | 四个分级：T1（仅元数据）到 T4（自主 + 网络访问） |
| **威胁分类** | 六种攻击类别 (AT1-AT6)，含 Code Trust Gate 嵌入式代码验证 |
| **程序生命周期** | Draft → Active → Deprecated → Archived，含迁移指南 |
| **互操作性** | 双向 MCP 工具映射、A2A Agent Card 集成、Pattern G 嵌入式运行时 |
| **治理哈希** | 对程序中所有 `.aisop.json` 文件的 SHA-256 完整性验证 |
| **AIAP 打包** | `.aiap` 归档格式，含清单、文件哈希和 Code Trust Gate |

## 架构

AIAP 通过三域联邦信任模型运行在 **AISOP**（AI 标准操作规程）格式之上：

```
┌─────────────────────────────────────────────────────┐
│                  AIAP 治理栈                         │
├─────────────────────────────────────────────────────┤
│                                                     │
│  aisop.dev (种子层)                                  │
│    定义 .aisop.json 格式规范                          │
│    中立、静态、基础性                                  │
│                         ↓                           │
│  aiap.dev (权威层)                                   │
│    定义治理规则、质量标准、                             │
│    安全约束和 Axiom 0 强制执行                         │
│                         ↓                           │
│  soulbot.dev (执行层)                                │
│    参考运行时，执行 AISOP 代码、                       │
│    解析工具、管理内存和权限                             │
│                                                     │
├─────────────────────────────────────────────────────┤
│  Axiom 0: 人类主权与福祉 (不可变)                      │
└─────────────────────────────────────────────────────┘
```

### AIAP 在 Agent 技术栈中的位置

```
┌──────────────────────────────────────┐
│  A2A 协议   — Agent 通信             │  Agent 之间如何对话
├──────────────────────────────────────┤
│  MCP 协议   — 工具访问               │  Agent 如何访问工具和数据
├──────────────────────────────────────┤
│  AIAP 协议  — 程序治理               │  Agent 程序如何被结构化、
│                                      │  验证和安全约束
├──────────────────────────────────────┤
│  AISOP 格式 — 蓝图语言               │  .aisop.json 执行格式
├──────────────────────────────────────┤
│  LLM 运行时 — 模型执行               │  Claude, Gemini, GPT 等
└──────────────────────────────────────┘
```

## 快速开始

- **协议规范**：阅读完整结构规则 [`specification/AIAP_Protocol.md`](specification/AIAP_Protocol.md)（[中文版](specification/AIAP_Protocol_cn.md)）
- **质量标准**：查看验证规则 [`specification/standards/`](specification/standards/)
- **概念指南**：浏览 [`docs/topics/`](docs/topics/) 了解各主题的通俗解释
- **实践指南**：跟随 [`docs/guides/`](docs/guides/) 中的分步教程
- **示例**：查看 [`examples/`](examples/) 中的 AIAP 程序示例
- **参考手册**：在 [`docs/reference/`](docs/reference/) 中查阅字段、规则和术语

## AIAP 程序结构

每个 AIAP 程序位于 `*_aiap/` 目录中，最小结构如下：

```
my_program_aiap/
├── AIAP.md            # 治理合约（身份、工具、模块、信任等级）
├── main.aisop.json    # 入口点（Pattern B+ 的意图路由器）
├── module1.aisop.json # 功能模块
├── module2.aisop.json # 功能模块
└── memory/            # 可选：状态持久化（Pattern E）
```

`AIAP.md` 文件是程序的治理合约，声明其协议合规性、工具、模块、信任等级和能力。它是人工审查和自动发现的入口点。

## 贡献

欢迎参与增强和推进 AIAP 协议！

- **问题与讨论**：使用 [GitHub Discussions](https://github.com/AISOP-AIAP-SoulBot/AIAP/discussions)
- **Issues 与反馈**：通过 [GitHub Issues](https://github.com/AISOP-AIAP-SoulBot/AIAP/issues) 报告
- **贡献指南**：查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解详情

## 许可证

本项目基于 Apache License 2.0 许可 — 详见 [LICENSE](LICENSE) 文件。

```
Copyright 2026 AIXP Foundation AIXP.dev | AIAP.dev
```

---

Align: Human Sovereignty and Benefit. Version: AIAP V1.0.0. www.aiap.dev
