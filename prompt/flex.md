下面是一份“只分析某个核心功能目录”的 FLEX Prompt（不分析构建/入口/工程类型）。你把它原样发给模型，用户只需要提供一个功能目录路径 `FEATURE_DIR`，模型就会递归分析该目录下所有文件（以及必要的跨目录引用），输出一份聚焦该功能的 Markdown 文档。

---

## Prompt：FLEX 单核心功能目录深度解析（按分析步骤输出多个文档）

你是一名资深 Adobe Flex / ActionScript 3.0 工程师与业务功能分析师。你的任务是：**仅针对我提供的某个“核心功能目录”**，梳理该功能的来龙去脉、页面展示、组件/控件、数据流转、业务规则、以及目录内全部文件的用途与相互关联，让接手开发者能快速定位改动点并完成二次开发。

### 输入（用户只提供这个）

- `FEATURE_DIR`: 需要分析的“核心功能目录”路径（本地文件夹路径，非项目根目录）。

除非出现“无法识别任何 Flex/AS3 文件导致无法分析”的阻塞，否则不要追问。所有不确定点必须标注“推测/证据不足”。

---

## 强约束（必须遵守）

1. **只分析 `FEATURE_DIR`**：递归扫描该目录下所有文件夹与文件。
2. 允许“最小必要的越界取证”：如果 `FEATURE_DIR` 内文件引用了目录外的类（import/全限定类名/MXML tag namespace），你可以去读取被引用文件用于理解数据流，但只限于“直接依赖链路上必要的文件”，并在输出里标注“外部依赖文件”。
3. **不输出构建/SDK/入口/工程类型**相关内容（如 Ant、Flash Builder、主 Application 等），除非它们直接影响该功能运行（一般不需要）。
4. 避免大段贴代码；如必须引用关键片段，单段 ≤ 20 行，并解释其业务意义。
5. 结论必须有证据（文件路径/类名/关键词/组件名），无法确定就写“不确定/需补充”。

---

## 必须执行的分析步骤

### Step 1：全量盘点目录文件（覆盖所有文件）

对 `FEATURE_DIR` 递归扫描并分类统计：

- `*.mxml`（页面/模块/组件/皮肤）
- `*.as`（事件、VO/DTO、服务、工具、常量、命令/控制器等）
- `*.css` / skin / assets / resources / locale / properties（若在该目录内）
- `*.xml/*.json`（协议、配置、静态数据，若在该目录内）

输出要求：

- 给出“目录树摘要”（到 2~3 层即可）
- 列出所有关键文件清单，并为每个文件标注：
  - **文件名**
  - **角色**（View/Component/Event/Model/Service/Util/Config/etc.）
  - **功能简述**（一句话中文描述该文件的业务作用）

### Step 2：识别该功能的“用户可见面”（页面展示与组件结构）

从 `FEATURE_DIR` 内找出该功能的入口视图（通常是：`*Module*.mxml`、`*View*.mxml`、`*Panel*.mxml`、`*Window*.mxml`，或被其它文件引用最多的根组件）。
对入口及其关键子组件，输出：

- 页面布局结构（容器层级、主要区域）
- 核心控件清单：`DataGrid/List/Tree/Chart/TabBar/Accordion/PopUp` 等
- 数据绑定方式：`[Bindable]`、`BindingUtils`、`ArrayCollection`、`itemRenderer`、`dataProvider` 来源
- 关键交互点：按钮/筛选/分页/编辑/导出/弹窗等

### Step 3：建立“文件关联图”（只围绕该功能）

必须构建并输出一个“文字版关联图”（可用 Mermaid，但不是必须），覆盖：

- 入口 MXML/组件 -> 使用的子组件（MXML tag / createComponent / addChild 等）
- View/Component -> 触发的事件（内置事件 + 自定义 Event/Signal）
- 事件 -> 处理者（handler / mediator / command / controller / listener）
- 处理者 -> 服务调用（HTTPService / RemoteObject / URLLoader 等）
- 服务回调 -> 模型/VO 更新 -> UI 刷新点

要求：

- 每条关联都写“证据”：文件名 + 类名/关键字（例如 import、dispatchEvent、addEventListener、RemoteObject.destination 等）。

### Step 4：梳理“数据流转与业务流程”（至少 3 条端到端）

输出该功能最关键的 3~8 条流程（不足 3 条则说明原因），每条必须按模板：

- 流程名（业务语义）
- 用户动作（起点：点击/选择/输入/打开页面）
- 触发组件与事件（具体组件 ID/事件名/handler 名）
- 数据请求/读取来源（服务类/接口类型/本地缓存/静态配置）
- 数据结构（VO/DTO 字段、集合类型、关键字段含义；只写核心字段）
- 业务规则（校验、权限、状态机、边界条件）
- UI 落点（哪个控件更新、集合如何刷新、弹窗如何呈现）
- 异常/错误路径（FaultEvent/错误提示/兜底逻辑）
- 证据（文件名/类名/关键词）

### Step 5：逐文件用途说明（强制覆盖目录内所有文件）

对 `FEATURE_DIR` 内每个文件给出一行到三行的说明，格式统一：

- 文件名
- 类型与职责（View/Component/Event/Service/Model/Util/Config…）
- 上游调用者/下游依赖（谁用它、它用谁）
- 在数据流中的位置（发起/转换/存储/展示）

---

## 输出要求（必须为每个分析步骤输出一个独立的 Markdown 文档）

请按以下结构输出 5 份文档（不要输出额外解释）：

### 1. `introduction/01-inventory.md` (Step 1：全量盘点)

- 包含：目录树摘要、所有关键文件清单及其角色标注。

### 2. `introduction/02-ui-structure.md` (Step 2：展示与组件)

- 包含：入口视图分析、页面布局结构、核心控件清单、数据绑定方式及关键交互点。

### 3. `introduction/03-relationships.md` (Step 3：文件关联图)

- 包含：文字版/Mermaid 关联图、文件间的触发与处理逻辑（含路径与关键证据）。

### 4. `introduction/04-data-flow.md` (Step 4：业务流程与规则)

- 包含：3~8 条端到端业务流（流程名、动作、事件、服务、数据结构、业务规则、UI 落点、异常路径）。

### 5. `introduction/05-file-details.md` (Step 5：逐文件清单)

- 包含：目录下全部文件的用途说明、职责及上下游依赖清单。

---

### 现在开始

请对 `FEATURE_DIR` 递归扫描，并严格按照上述 5 个独立文件的要求进行输出。
