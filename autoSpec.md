## 任务：执行 Flask 代码质量工作流

**审计目标目录：** ./

**执行要求：立即按序执行以下步骤，不要询问，直接开始！**

### 🚨 强制执行规则：
- **步骤 1、2、3、5 必须使用 task 工具创建 subagent**，类型：general-agent
- **步骤 4 不使用 subagent**，由主 Agent 直接执行（接口文档生成依赖前三步产物）
- **一个 subagent 只执行一个步骤**，完成后返回结果再创建下一个
- **严格按序号执行**：0 → 1 → 2 → 3 → 4 → 5

---

**步骤 0: 初始化工作流**
subagent 任务：调用 TodoWrite 初始化工作流任务列表

**步骤 1: 代码规范检查**
subagent 任务：执行 spec go，扫描当前目录下所有 .py 文件，检查 PEP8 规范
subagent 内调用：RunCommand (使用 pylint 或 ruff 进行代码规范检查)

**步骤 2: 注释质量检查**
subagent 任务：执行 spec go，检查所有函数、类、模块的 docstring 完整性
subagent 内调用：RunCommand (使用 pylint-docstrings 或类似工具检查文档完整性)

**步骤 3: 路由安全审计**
subagent 任务：执行 spec go，审计所有 Flask 路由，检查鉴权、输入验证、错误处理
subagent 内调用：RunCommand (使用安全扫描工具如 bandit)

**步骤 4: 接口文档生成**
⚠️ 此步骤不需要创建 subagent！
由主 Agent 直接执行 spec go，基于路由扫描结果生成 OpenAPI 3.0 文档
直接调用：RunCommand (使用 flasgger 或 apispec 生成 OpenAPI 文档)

**步骤 5: 汇总质量报告**
subagent 任务：执行 spec go，汇总前四步结果，生成最终质量评分报告
subagent 内调用：RunCommand (汇总各工具输出结果)
