# frontend-best-practices

`frontend-best-practices` 是一个用于前端交互开发和交付前自检的 Codex skill。

它不是视觉设计生成器，也不是某个框架的组件库规则，而是一个前端质量守门员：在实现或修改用户界面时，先区分 Web 端、小程序端、App 端或混合端，再提前覆盖真实用户常遇到的小坑，包括重复提交、慢接口、局部 loading、错误态、空态、权限态、长文本、响应式、表单校验、列表刷新、平台验证边界，以及后台产品界面的克制风格。

## 安装

从 GitHub 安装：

```bash
npx skills add https://github.com/glm1024/frontend-best-practices -g -y
```

## 更新

如果是按上面的全局方式安装，后续更新：

```bash
npx skills update frontend-best-practices -g -y
```

如果是项目级安装，把 `-g` 改成 `-p`，或在对应项目目录下运行更新命令。

维护这个仓库时，流程是：

1. 修改 `SKILL.md`、`references/` 或 `agents/openai.yaml`。
2. 本地检查 skill 是否能被识别：

   ```bash
   npx skills add /Users/mark/workspace/skills/frontend-best-practices -l
   ```

3. 提交并推送到 GitHub。别人再运行 `npx skills update frontend-best-practices -g -y` 就能拿到更新。

## 这个 skill 提供什么

- 前端改动前的范围判断和工作流：什么时候只做窄修，什么时候需要先出设计方向
- Web / 小程序 / App 的平台约束判断：不同端的交互、权限、生命周期、安全区和验证方式不能混用
- 异步交互安全规则：重复点击、防重入、提交态、行级动作锁、慢接口、 stale response
- 表单、弹窗、抽屉、表格、筛选、分页、上传、导入、批量操作等常见 UI 的状态完整性检查
- 后台 / 产品型界面的审美约束：克制、实用、信息密度合适，不把管理系统做成营销页
- 中文产品文案约束：按钮和确认文案要说清楚动作和后果，避免泛泛的“确定”“提交”
- 交付前 review checklist：状态、边界、响应式、可访问性、性能、项目风格、验证命令
- 浏览器验证边界：什么时候代码检查和构建够用，什么时候必须真实交互验证

## 目录说明

- `SKILL.md`：skill 入口，负责触发条件、核心工作流和 reference 路由
- `agents/openai.yaml`：Codex / OpenAI skill UI 元数据
- `references/workflow.md`：前端改动工作流、范围控制和验证策略
- `references/platform-surfaces.md`：Web、小程序、App 和混合端的平台约束与验证边界
- `references/interaction-states.md`：异步交互、表单、表格、上传、乐观更新和可访问性规则
- `references/product-ui-taste.md`：产品后台界面 taste、中文文案和视觉克制规则
- `references/review-checklist.md`：交付前审查清单和 review 严重级别

## 注意事项

- 这个 skill 不替代具体项目里的设计系统、组件库、`AGENTS.md` 或产品说明。
- 开发时仍然应该优先遵循当前项目已有组件、样式、接口封装、权限模型和验证脚本。
- 对后台 / 管理系统，默认做实用、克制、稳定的产品 UI；不要默认引入营销页、装饰性渐变、大 hero、卡片套卡片或过度动效。
- 对“按钮可重复点击”“接口慢时能连点”“loading 很突兀”这类问题，要同时处理 UI 状态和方法入口防重入，不能只禁用按钮。
- 对“页面不好看”“效果一般”这类反馈，先判断是局部 polish 还是方向问题；方向问题应先给 3 到 5 个方案，再落代码。
- 交付前至少做一次轻量验证，例如 `git diff --check`、项目构建、lint/typecheck，或在需要时进行真实浏览器交互验证。
