---
onenote-id: 88c7cde5-e655-4427-b005-2f64414cb46e:1
---
1. **高级功能**

**Shift+Tab****：切换权限模式**

- **Normal****：**每一步操作都会询问，最安全
- **Plan****：**先讨论方案，人工同意后才开始处理
- 在plan Model的时候，终端被占用，干不了别的。
- /Ultraplan把规划搬到了云端，Claude会把任务交给云端的多Agent去规划。三种方式可以触发：
- /Ultraplan
- 在prompt里加上ultraplan这个关键词
- 在plan mode 生成方案后选择【用Ultraplan在云端细化】

注意：第三方模型不支持这个模式，因为它需要在Anthropic的云端基础设施上跑

- **Auto****：**全权放手
- **acceptEdits****：**Claude修改文件自动通过，跑命令还是会询问人工。在/config里可以切换到这个模式

**/context****：**查看当前使用的上下文数量

**/compact****：**压缩历史上下文

**/clear****：**清空会话

**/cost****：**查看当前消耗

**/rename****：**给当前对话重命名

**/resume****：**继续之前的某次对话

**/status****：**查看当前的使用状态，比方token消耗

**/statusline****：**定制状态栏

**/usage****：**查看使用量

**/****rewind****：**支持回滚对话、回滚代码、两者都回滚

**/goal****：**设个目标让它自己跑完

**/recap****：**总动帮你总结上下文（有事中断了会话，忘记刚刚对话进行到哪了）

**/loop****：**让Claude跑定时任务

1. **高频输入技巧**
- @：用 @ 引用相关文件
- **!****（英文格式）：**变成shell模式
- **/btw****：**在执行中，突然想要提问，又不想污染上下文
- **双击****esc****：**当你在Claude code里面输入日志等很多信息的时候，你想全部删除，这个时候如果你一直按回车键会崩溃的，直接双击esc就行了。
- **粘报错：**代码跑挂了、命令报错了，把终端输出整段粘进对话框，加一句「修一下」。Claude 会读报错、定位文件、改代码。大多数情况下比你自己 Google 快。

让Claude先问清楚再动手（比方：xxxx，在动手之前，先问我所有你需要搞清楚的问题。  问完让它整理成 Spec 文档，然后开一个新会话，把 Spec 喂给新的 Claude 来执行。）

在prompt里加上ultrathink这个关键词，可以让claude进行深度思考

1. **急救三步**

Claude code用到一定阶段一定会卡

- ESC 中断当前操作
- /clear 清空当前会话
- 重新描述任务，这次写得更具体
1. **高级功能**
- 启动时加上 –dangerously-skip-permissions 参数， Claude Code 就获得了完全终端权限，所有操作不再询问
- 分层配置
- 可以在不同层级下编写claude.md
- /hooks：让 Claude Code 在特定时机自动执行任务
- Skills：在 ~/.claude/skills/ 目录下创建 skill.md 文件，定义任务的标准流程
- 使用 Skill 有两种方式：Claude Code 自动匹配，或者你用 /skill-name 显式调用。
- Sub-Agent：/agent 创建一个独立的 Agent
- 它在自己的空间里完成任务，只把最终结果返回给主对话。
- Plugin：/plugin 进入插件管理器

**/plugin marketplace add anthropics/claude-plugins-official****官方插件**

1. **Auto Memory**

Claude Code 有一个 Auto Memory 机制。它在工作过程中会把它认为重要的东西自动记下来，比如你项目的构建命令、你偏好的代码风格、它踩过的坑和解决方案。其中用户记忆存在 ~/.claude/CLAUDE.md 里（就是全局级那个文件），工程记忆存在./CLAUDE.md 里面，下次开新会话的时候自动加载。

你可能用了几天之后打开 ~/.claude/CLAUDE.md，发现里面多了一堆你没写的东西。不用慌，那是 Claude 自己记的。

输入 /memory 可以查看和编辑这些自动记忆。建议隔一阵看看，删掉过时的，保留有用的。

1. **Hooks**

Hooks 是 Claude Code 平台层面的机制，在特定生命周期节点触发 Shell 脚本，Claude 无法跳过或忽略。

现在的hooks已经新增了很多功能

- PreToolUse：工具调用之前拦截危险操作
- PostToolUse：编辑之后自动化或测试
- Stop：回合结束时推进继续执行
- PostCompact：压缩上下文后重新注入关键提醒

![[Claude code2 image.png]]

1. **多****agents**
1. **Subagents**

每个子代理有自己独立的 Context、独立的任务、独立的工作目录。

在 .claude/agents/ 目录下放一个 .md 文件，就定义了一个子代理

1. **Git Worktrees**

**claude --worktree****：**每次运行这个命令，Claude Code 会自动创建一个新的 worktree、切到一个新分支，在隔离环境中工作。完成后把分支合并回主分支。

**claude --worktree --tmux****：**在后台运行

后台任务管理启动开发服务器（npm run dev）后，服务会阻塞 Claude Code 的交互。按 Ctrl + B 可以把服务放到后台运行。输入 /tasks 查看所有后台任务，按 K 键终止选中的任务。这样就能同时运行多个长期进程，不影响正常使用。版本回滚及其局限性按 –rewind 或双击 ESC 可以进入回滚界面，选择回滚到某个历史状态。但要注意：Claude Code 只能回滚它自己写入的文件。通过终端命令创建的文件（比如 mkdir 建的目录、npm install 装的依赖）无法回滚。所以 Git 还是必不可少的。

作者：大模型爱好者社区

链接：https://zhuanlan.zhihu.com/p/2031489748343185542

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

作者：AI一手

链接：https://zhuanlan.zhihu.com/p/2041162625233511147

来源：知乎

著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

**命令**

**作用**

/status

看当前会话配置（模型、目录、Token）

/context

看 Context 用量

/clear

清空当前会话

/compact

压缩当前会话

/init

在当前目录生成 CLAUDE.md

/cost

看当前会话消耗

/model

切换模型

/permissions

管理权限白名单

/config

打开交互式设置面板（别名 /settings）

/btw

侧链提问，不污染当前上下文

/doctor

环境诊断，检查安装、API、MCP 等 20 多项

/recap

回到一个中断的会话时自动总结上下文

/goal

设目标让 Claude 跨轮次自动干到完成

/team-onboarding

根据你的使用习惯生成团队入职指南

/voice

进入语音模式，按住空格说需求

/ultraplan

把规划任务交给云端多 Agent，浏览器审阅

/ultrareview

云端多 Agent 审查代码，找真 bug

/memory

查看和编辑 Claude 的自动记忆

/loop

定时循环执行任务或设一次性提醒

ultrathink

在 Prompt 中加入，让 Claude 深度推理

claude -p

非交互模式，可接入脚本和 CI/CD

Shift+Tab

切换 Normal / Plan / Auto 模式

Esc

中断当前操作

Esc × 2

打开 Rewind 菜单（回滚对话/代码）

@文件名

引用文件给 Claude 读

!命令

在对话框里直接跑 shell 命令

![[Claude code2 image 1.png]]