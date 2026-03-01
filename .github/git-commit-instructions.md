# Git Commit Message Instructions

本文件用于约束 Git Commit Message 的生成规则，
主要配合 GitHub Copilot 的 Commit Message Generation 使用。

---

## 1. 基本语言规则

- Commit Message **使用中文**
- 可以包含必要的英文技术词（函数名、模块名、库名）
- 不要中英文混乱堆砌，描述应简洁明确

---

## 2. Commit Message 格式

统一格式：

<type>: <简短描述>

示例：

feat: 新增用户登录接口
fix: 修复订单价格为空导致的崩溃
refactor: 重构支付模块结构

规则说明：

- **必须是单行**
- 不要换行
- 不要追加解释性段落
- 描述控制在 **72 个字符以内**

---

## 3. Commit Type 规范（Conventional Commits）

允许使用以下 type（不限于 feat / fix）：

- feat:     新功能
- fix:      修复 Bug
- refactor: 重构（不改变外部功能）
- perf:     性能优化
- test:     测试相关
- docs:     文档更新
- chore:    构建、工具、配置、杂项
- ci:       CI / CD 相关
- build:    构建系统或依赖变更
- style:    代码风格调整（不影响逻辑）

> 如果无法明确判断类型，**优先使用 `chore`**

---

## 5. 描述内容要求

- 使用 **祈使句**
- 描述“做了什么”，而不是“为什么”
- 避免使用模糊动词：
    - ❌ update
    - ❌ change
    - ❌ modify
- 推荐使用：
    - add / remove / fix / refactor / optimize / simplify

示例对比：

update user logic
fix: 修复用户状态判断错误

---

## 6. Copilot 使用约定（重要）

当使用 GitHub Copilot 生成 Commit Message 时：

- 必须严格遵循本文件规则
- 只输出 **最终可用的 commit message**
- 不要输出解释、分析或多条候选

---

## 7. 正确示例汇总

feat: 新增 JWT 刷新机制
fix: 修复空列表导致的索引越界
refactor: 重构订单服务目录结构
docs: 补充接口异常返回说明
chore: 调整 GoLand 编译参数

---

## 8. 错误示例（不要这样）

feat: update code
fix bug
修改了一些问题
重构代码并修复多个 bug（过于模糊）

---

## 9. 设计文件（*.pen）特殊规则

对于 Pencil 设计文件（*.pen），需要额外遵循以下规则：

### 9.1 设计操作的 Commit Type 映射

| 设计操作 | 推荐类型 | 示例 |
|---------|---------|------|
| 新增屏幕/页面 | feat | feat: 新增登录页面设计 |
| 修改组件样式 | feat | feat: 更新按钮组件样式 |
| 删除元素 | feat | feat: 移除废弃的导航栏 |
| 调整布局 | refactor | refactor: 优化主页布局结构 |
| 修复设计错误 | fix | fix: 修复卡片对齐问题 |
| 更新变量/主题 | style | style: 调整全局配色方案 |
| 添加设计系统组件 | feat | feat: 新增表格组件 |

### 9.2 设计描述规范

描述应包含：
- **操作对象**：屏幕、组件、元素名称
- **变更内容**：新增、修改、删除、调整
- **设计特征**：布局、样式、交互、主题

示例：

feat: 新增用户详情页卡片布局
refactor: 重组仪表盘侧边栏结构
style: 统一按钮圆角和间距
fix: 修复弹窗层级遮挡问题

feat: update design（过于模糊）
style: 修改了一些样式（缺少具体说明）

### 9.3 批量操作说明

当一次设计涉及多个页面或组件时：
- 描述应体现**整体目标**而非逐个列举
- 使用概括性术语：页面组、模块、区域

示例：

feat: 重构用户模块所有页面布局
refactor: 统一表单组件交互规范

---

## 10. 最终原则

> Commit Message 是给 **未来的自己和协作者看的**
> 清晰、稳定、可追溯，比"看起来高级"更重要

> **对于设计文件**：描述变更对设计系统的影响，而非视觉细节