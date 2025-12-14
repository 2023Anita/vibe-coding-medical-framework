# Vibe Coding x Claude Code：项目构建实战框架（医疗版）

<p align="center">
  <strong>🏥 专为医疗AI项目设计的Claude Code结对编程框架</strong>
</p>

<p align="center">
  <a href="#-快速使用指南">快速使用</a> •
  <a href="#-五阶段开发流程">开发流程</a> •
  <a href="#-项目示例">项目示例</a> •
  <a href="#-核心原则">核心原则</a>
</p>

---

本框架专为使用 **Claude Code (CLI)** 的开发者设计。它将《Vibe Coding》的"道、法、术"转化为具体的操作步骤，确保你在命令行中与 AI 结对编程时，保持项目结构清晰，避免"热力学熵增"（代码混乱）。

## 🚀 快速使用指南

### 推荐工具

| 工具 | 推荐度 | 说明 |
|------|--------|------|
| [反重力](https://www.antigravity.dev/) | ⭐⭐⭐ | 强烈推荐！免费使用 Opus 4.5，集成 Banana 生图功能 |
| [光标](https://cursor.sh/) | ⭐⭐ | 主流 AI 编辑器，支持多种模型 |
| [VS Code + Copilot](https://code.visualstudio.com/) | ⭐⭐ | 微软官方方案 |
| [Claude Code](https://docs.anthropic.com/en/docs/claude-code/overview) | ⭐⭐ | Anthropic 官方 CLI 工具 |

### 三步开始

```bash
# 1️⃣ 克隆仓库
git clone https://github.com/2023Anita/vibe-coding-medical-framework.git

# 2️⃣ 打开聊天窗口
# 在编辑器中打开 AI 聊天面板 (Antigravity/Cursor/Copilot Chat)

# 3️⃣ 开始对话
# 直接告诉 AI 你想创建什么内容，AI 会自动按照仓库中的角色定义工作
```

---

## 📋 五阶段开发流程

### 🚀 第一阶段：定锚（上下文构建）

> **核心原则**：上下文是第一性要素；垃圾进，垃圾出。

在敲入任何代码指令之前，你必须先建立一个"项目宪法"文件（`PLAN.md` 或 `CONTEXT.md`）。

**PLAN.md 模板示例**（以麻醉术前访视AI系统为例）：

```markdown
# 项目：Anesthesia-PreOp-AI (麻醉术前访视辅助系统) 核心规划

## 1. 一句话目标 (Objective)
> 构建一个基于 Python 的辅助系统，通过分析患者电子病历 (EMR) 和检验单，
> 自动生成 ASA 分级建议、麻醉风险预警及标准化的术前访视单草稿。

## 2. 非目标 (Non-Goals)
> - **严禁**：系统仅作辅助，不代替医生做最终医疗决策或下达医嘱。
> - **范围外**：不处理急诊手术（仅限择期手术），不直接连接医院生产环境数据库。
> - **不做**：暂不开发移动端 App，仅提供 Web 端管理后台。

## 3. 系统性思考 (System Design)
| 维度 | 内容 |
| :--- | :--- |
| **实体 (Entities)** | Patient (患者), LabResult (检验结果), Comorbidity (合并症), AnesthesiaPlan (麻醉计划) |
| **链接 (Links)** | Patient 拥有多个 LabResult；Comorbidity 影响 ASA 分级 |
| **核心功能** | IngestData (数据摄入), RiskCalc (风险计算), GenReport (报告生成) |

## 4. 技术栈约束
- 语言: Python 3.10+
- 核心库: LangChain (逻辑编排), Pydantic (数据验证), Streamlit (医生交互界面)
- 风格: Google Docstring, Type Hinting (医疗数据严谨性要求高)
```

**初始化 Claude 会话**：

```bash
claude
> /add PLAN.md
> "阅读 PLAN.md。这是本项目的核心规划。请注意这是一个医疗辅助系统，
>  对数据准确性和逻辑严谨性有极高要求。在后续的所有对话中，请严格遵守
>  其中的约束和目标。如果理解了，请回复'麻醉AI系统规划已加载，随时待命'，
>  不要生成代码。"
```

---

### 🏗 第二阶段：骨架（先结构，后代码）

> **核心原则**：凡是 AI 能做的，就不要人工做；先结构，后代码。

**步骤1：让 AI 设计结构 (Thinking)**

```
"基于 PLAN.md 的系统设计，请为我设计一个模块化的项目目录结构。
要求：
1. 遵循'正交性'原则：数据定义 (models)、核心逻辑 (core)、界面 (ui) 要分离。
2. 包含 mock_data 目录用于存放脱敏的测试数据。
3. 输出 ASCII 树状图，并解释每个主要文件/文件夹的用途。
4. 此时**不要**创建任何文件，先让我确认结构。"
```

**预期结构输出**：

```
anesthesia-preop-ai/
├── src/
│   ├── models/          # Pydantic 数据模型 (Patient, LabResult)
│   ├── core/
│   │   ├── parser.py    # EMR 数据解析
│   │   ├── risk.py      # ASA 分级与风险计算核心逻辑
│   │   └── report.py    # 报告生成器
│   └── ui/              # Streamlit 界面代码
├── data/
│   └── mock_patients/   # 测试用 JSON 数据
├── tests/               # Pytest 测试用例
├── PLAN.md
└── requirements.txt
```

**步骤2：一键落地骨架 (Action)**

```
"结构看起来不错。请执行 Shell 命令：
1. 创建上述所有文件夹。
2. 为每个 Python 文件创建空白文件（使用 touch），如果是文件夹则确保有 __init__.py。
3. 创建 .venv 虚拟环境 (python -m venv .venv)。
4. 创建一个基础的 requirements.txt，包含 pydantic, streamlit, pytest, langchain。"
```

---

### 🧩 第三阶段：填充（接口先行，实现后补）

> **核心原则**：接口先行；一次只做一件事；专注。

**步骤1：定义核心数据实体 (Models)**

```
"首先开发 src/models/patient.py。
请基于 Pydantic 定义核心数据模型：
1. `LabResult`: 包含项目名称、数值、单位、参考范围。
2. `Patient`: 包含姓名(脱敏)、年龄、性别、既往史 (List[str])、检查结果列表 (List[LabResult])。
3. 请只写代码，确保包含类型注解和详细的 Docstring。"
```

**步骤2：定义核心逻辑接口 (Signatures)**

```
"接下来定义 src/core/risk.py 的接口。
引用刚才定义的 Patient 模型。
定义一个函数 `calculate_asa_score(patient: Patient) -> dict`。
要求：
1. 只写函数签名和 Docstring。
2. Docstring 中详细描述输入输出，以及 ASA I-V 级的判断标准。
3. 函数体内目前只写 `pass`。"
```

**步骤3：逐个模块实现 (Implementation)**

```
"现在实现 `calculate_asa_score` 的具体逻辑。
规则参考：
- 健康患者 -> ASA I
- 轻度系统性疾病 (如控制良好的高血压) -> ASA II
- 严重系统性疾病 (如心梗历史 > 3个月) -> ASA III
- 威胁生命的疾病 -> ASA IV
请编写代码替换 pass。"
```

---

### 🐞 第四阶段：调试与验证（逆向思考）

> **核心原则**：Debug 只给预期 vs 实际；测试交给 AI。

**步骤1：生成测试数据与用例**

```
"在 tests/test_risk.py 中生成测试用例。
1. 创建一个 Mock 的 ASA II 级患者数据（例如：控制良好的糖尿病）。
2. 创建一个 Mock 的 ASA III 级患者数据（例如：既往有心肌梗死）。
3. 编写 pytest 测试函数验证 `calculate_asa_score` 是否返回正确结果。"
```

**步骤2：自动运行与修复 (Loop)**

```
"请运行 pytest。"
```

如果测试失败，描述问题：

```
"测试失败了。
预期：心梗患者应该是 ASA III 级。
实际：代码返回了 ASA II 级。
请检查 risk.py 中的逻辑，是不是漏掉了对'心梗'关键词的检测？修复它。"
```

---

### 🧹 第五阶段：维护（会话管理）

> **核心原则**：上下文是第一要素；代码一多就切会话。

**步骤1：代码提交 (Save points)**

```
"代码运行正常。请执行 git add 和 git commit，提交信息为 'feat: 完成 ASA 分级核心逻辑'。"
```

**步骤2：上下文压缩 (Context Management)**

- 输入 `/compact` 命令：Claude 会总结之前的对话历史，释放 Token 空间。
- 开始新模块时，建议重启会话：
  1. 输入 `exit` 退出
  2. 重新输入 `claude` 进入
  3. 输入 `/add PLAN.md src/models/patient.py src/core/risk.py` (只加载必要文件)

---

## ⚡ 完整指令流总结

| 步骤 | 指令 | 说明 |
|------|------|------|
| 1. 启动 | `claude` → `/add PLAN.md` | 加载项目规划 |
| 2. 骨架 | "设计目录结构" → "执行 mkdir/touch 命令" | 创建项目骨架 |
| 3. 模型 | "定义 src/models/patient.py (Pydantic)" | 定义数据结构 |
| 4. 接口 | "定义 src/core/risk.py 接口 (pass)" | 定义函数签名 |
| 5. 实现 | "实现 calculate_asa_score 逻辑" | 填充具体代码 |
| 6. 测试 | "生成并运行 pytest" → "修复 Bug" | 验证功能 |
| 7. 收尾 | "git commit" → `/compact` → 下一个模块 | 保存并继续 |

---

## 🎯 核心原则

| 阶段 | 核心原则 |
|------|----------|
| 定锚 | 上下文是第一性要素；垃圾进，垃圾出 |
| 骨架 | 凡是 AI 能做的，就不要人工做 |
| 填充 | 接口先行；一次只做一件事 |
| 调试 | Debug 只给预期 vs 实际；测试交给 AI |
| 维护 | 代码一多就切会话 |

---

## 📄 原始PDF文档

完整的框架说明请参考：[项目构建实战框架-医疗版.md](项目构建实战框架-医疗版.md)

---

## 📝 License

MIT License

---

<p align="center">
  <sub>Made with ❤️ for Medical AI Developers</sub>
</p>
