## 一、CC中使用Agent skill
先开门见山，让你快速用起来，建立学习自信，同时降低认知成本。
### 1.1 创建skill目录
```
mkdir -p ~/.claude/skills/explaining-code
```
### 1.2 创建SKILL.md文件
```
---
name: explaining-code
description: 使用可视化图表和类比来解释代码。在解释代码工作原理、教授代码库知识，或用户询问"这是如何工作的？"时使用。
---
解释代码时，始终包含：
1. **从类比开始**：将代码与日常生活中的事物进行比较
2. **绘制图表**：使用 ASCII 艺术展示流程、结构或关系
3. **逐步讲解代码**：逐步解释发生了什么
4. **指出常见陷阱**：常见的错误或误解是什么？
保持解释的对话性。对于复杂概念，使用多个类比。
```
### 1.3 检查可用性
启动claude code，然后执行/skills可查看当前所有skill
![](https://i-blog.csdnimg.cn/img_convert/8e1ed79145a149351478eafc90f43bfb.png)
当然官方提供了一种验证方法，`What Skills are available?`，不过是一种提问表述，你用中文也可以。
![](https://i-blog.csdnimg.cn/img_convert/28139e1fd53c9bb6d420fbc7e53dd573.png)
### 1.4 测试效果
测试效果，本质就是给AI一个符合SKILL要求的任务
比如我开发了一个格式化文本的SKILL，如下图，我告诉他格式化清理空格。
**我的SKILL**
![](https://i-blog.csdnimg.cn/img_convert/e234a52c74c0868db8935bcc3e267f6d.png)
**实际测试**
![](https://i-blog.csdnimg.cn/img_convert/6da4dedc1b831d8a3a8004f3d29a2a97.png)
![](https://i-blog.csdnimg.cn/img_convert/f7ec21949afe1a7a16c2c2ffe698d6d7.png)
从上面我们会发现一个有趣的问题，**第一次它没有使用技能，当我问它后，它调用了技能**。
这里其实可以得到2个结论：
- **SKILL不是100%执行的**，官方也说明了，符合要求才会触发技能
- **过于简单的任务，必须要触发技能**，AI自己可以处理
按照上面流程，用户可以快速使用和验证，掌握Agent Skill使用要领，**构建学习信心**，然后下面我们在细节展开知识点。
## 二、一个真实案例实践
### 2.1 安装
安装方式有多种，我这里只针对CC的安装进行介绍。
**方法一：** 简单粗暴，直接下载官方github文件，然后复制到对应目录下。
https://github.com/anthropics/skills
![](https://i-blog.csdnimg.cn/img_convert/10f7c7821e209c6f7c61f287fe4a0d28.png)
**方法二：** 注意颜面，保持优雅，用官方命令进行安装
- 启动claude执行 `/plugin install document-skills@anthropic-agent-skills`
- 完成后重启项目，如果不行，执行 `/plugin` 检查是否启用
**方法三：** 兜兜绕绕也能行，从插件里选择，还可以看到别的skill平台
- 启动claude执行 `/plugin marketplace add anthropics/skills `
![](https://i-blog.csdnimg.cn/img_convert/5c85f5ef44522cb2cd282bf48934eb5e.png)
- 执行`/plugins`，在 Discover中找到官方skill
![](https://i-blog.csdnimg.cn/img_convert/5e58c617549bc957ebaaee2ba4ffa2e8.png)
- 选择安装位置，用户维度，项目维度，本地当前库，安装完成后，会在`.claude/`下生成一个json文件
![](https://i-blog.csdnimg.cn/img_convert/b5a37d90daea3b5254654cd05235da66.png)
- 重启claude 工具，再次执行 `/skills`，即可查看到安装的技能
![](https://i-blog.csdnimg.cn/img_convert/e31d81f0ffe9080346842b48b3c8c0f9.png)
### 2.2 实际应用
- 我让claude把excel生成数据报表，这个表述命中了技能描述，所以开始调用技能
```
---
name： xlsx
description： 全面的电子表格创建、编辑和分析功能，支持公式、格式化、数据分析和可视化。当 Claude 需要处理电子表格文件（.xlsx、.xlsm、.csv、.tsv 等）时，可用于：1.创建包含公式和格式的新电子表格;2.读取或分析数据;3.修改现有电子表格同时保留公式;4.在电子表格中进行数据分析和可视化;5.重新计算公式
---
```
- 这里开始执行技能（会自动检测工具，如果没有安装，可以让claude自行安装处理）
![](https://i-blog.csdnimg.cn/img_convert/067f9bbfb8b35a0be1dd00449d73eb6b.png)
- 最终把我的excel中的数据提起生成了折线图和柱状图
![](https://i-blog.csdnimg.cn/img_convert/4781193ee58a7e15a4ee1a0b1e939a11.png)
### 2.3 哪里有别人做好的skill包
**官方skill**
https://github.com/anthropics/skills/tree/main/skills
**三方平台**
https://skillsmp.com/zh
**面向编程**
https://github.com/vercel-labs/agent-skills/tree/main/skills
https://github.com/obra/superpowers/tree/main/skills
https://github.com/ComposioHQ/awesome-claude-skills
**面向UI**
https://github.com/nextlevelbuilder/ui-ux-pro-max-skill
**工作流平台（dify/n8n）**
https://github.com/langgenius/dify/tree/main/.claude/skills
https://github.com/czlonkowski/n8n-skills
https://github.com/kepano/obsidian-skills
**其他类型**
https://github.com/OthmanAdi/planning-with-files
https://github.com/yusufkaraaslan/Skill_Seekers
## 三、基础信息
### 3.1 背景
智能体越来越强大，工作中需要更具可组合性、可扩展性和可移植性的方法实践
### 3.2 Agent skill是什么
一种**利用文件和文件夹构建专业化Agent的新方法**，用来扩展Claude功能的模块化功能。每个技能都包含指令、元数据和可选资源（脚本、模板）
- 指令：告诉Claude如何完成任务
- 元数据：描述Skill的作用和使用时机
- 资源：脚本、模板、参考文档等
可以理解成工具箱，每个工具箱里有不同工具， 医药箱里面放的药品，可以用来治病。工具箱里面放的扳手，可以用来拧螺丝，这些工具都被你所用。
![](https://i-blog.csdnimg.cn/img_convert/f5b16d3f0d77e498b5d5f58e78bca348.jpeg)
### 3.3 为什么使用
- 技能是基于**文件系统**的**可重用资源**，为Claude提供特定领域的专业知识和能力
- 相比rule能力更加强大，skills是更复杂通用能力封装（支持脚本，工具调用），不在仅仅局限Prompt
- 技能**按需加载**，无需在多个对话中重复提供相同的指导，与提示（针对一次性任务的对话级指令）不同
### 3.4 什么场景使用skill
- 发现自己在向AI反复解释同一件事，或者你在开发中，已经生成的模版结构，rule内容很可能有适用的场景
- 某些任务需要特定知识、模板、材料才能做好，但缺“特定场景的知识材料”（checklist文档/兼容性文档）
- 发现任务复杂，需要一些脚本工具，多个流程协同完成，`提取 => 清洗 => 组装 => 输出结果`，需要“组合多个流程”才能完成
### 3.5 整体架构
**架构来自于官方：**
https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
![](https://i-blog.csdnimg.cn/img_convert/9432a2c4e2df3eafad4f9acb568a460c.png)
### 3.6 技能的整体组成
- 最简单版本只要一个SKILL.md即可，只需要包含**元数据和指令**
- 复杂的则是由 元数据 + 指令 + 资源和代码三部分构成
- SKILL.md **不可以直接在 skills目录下，必须有子目录**
#### 3.6.1 skill位置
```
// 项目下
.claude/skills/
.claude/skills/demo1/SKILL.md
.claude/skills/demo2/SKILL.md
// 用户维度
～/.claude/skills/
// 插件下
skills/my-skill/SKILL.md在插件目录中
// 错误示例
～/.claude/skills/SKILL.md
```
#### 3.6.2 skill全部可用字段
**更多可以参考：**
https://agentskills.io/specification#body-content
https://code.claude.com/docs/en/skills#available-metadata-fields
![](https://i-blog.csdnimg.cn/img_convert/2d94e654eca2abfdd8fa3679ce555357.png)
#### 3.6.3 元数据（始终加载）
SKILL.md文件头部对技能的描述信息，**即为元数据**。SKILL.md YAML frontmatter 必须的只有两个name和description字段：
```
name：
最多 64 个字符
必须仅包含小写字母、数字和连字符。
不能包含 XML 标签
不能包含保留字：“anthropic”、“claude”
description：
必须不为空
最多 1024 个字符
不能包含 XML 标签
应该描述该技能的作用以及何时使用。
# 你的技能名字
## 介绍
## 示例
```
**以下一个简单的示例**
```
// SKILL.md
---
name: text-formatter
description: 格式化和美化文本内容，包括大小写转换、空格清理、换行处理和文本对齐。当用户需要格式化文本、清理空格或转换大小写时使用。
---
```
#### 3.6.4 指令（触发时加载）
也就是SKILL.md的主体内容，是对这个技巧的说明，让Claude知道要干什么。主体内容包含程序性知识，工作流程、最佳实践和指导原则
当您任务内容与技能描述相符时，Claude会通过bash从文件系统中读取SKILL.md文件。只有这样，该文件的内容才会显示在上下文窗口中**（节省Token和上下文）**。
**以下一个简单的示例**（这么简单的任务不需要SKILL，仅为示意）
```
// SKILL.md
---
name: text-formatter
description: 格式化文本内容，进行大小写转换
---
# 文本格式化工具
## 概述
这个 Skill 帮助用户对文本内容进行格式化操作。
## 支持的操作
- 标题格式：每个单词首字母大写
## 操作指南
1. 识别用户请求的格式化操作
2. 相应地处理输入文本
3. 除非明确要求删除，否则保留重要的格式
4. 返回格式化后的结果
## 示例
### 示例1：标题格式
输入：
the quick brown fox jumps over the lazy dog
操作：转换为标题格式
输出：
The Quick Brown Fox Jumps Over The Lazy Dog
## 使用提示
- 注意特殊字符和标点符号的处理
- 对于标题格式，考虑冠词（如 a、an、the）在中间时应该小写
- 如果操作不明确，请询问用户澄清
## 错误处理
如果输入为空：
- 返回空字符串并附注说明
如果操作不明确：
- 请用户指定操作
- 提供可用操作的示例
```
#### 3.6.5 资源和代码（按需加载）
主要包含如下三点：说明、代码、资源
**说明：** 包含指导和工作流程的附加Markdown文件（FORMS.md/REFERENCE.md）
**代码：** scripts脚本，通过bash运行的可执行脚本，这些脚本提供确定性操作，而无需消耗上下文信息
**资源：** 参考资料，例：API文档、输出模板
```
demoskill/
├── SKILL.md
├── FORMS.md (表单填写指南)
├── REFERENCE.md (详细的API参考文档)
└── scripts/
└── script.py (脚本)
```
**加载实际和消耗**
```
元数据 => 始终加载（启动时）=> 每项技能约需100 Token
说明 => 技能触发时 => 低于5000 Token => SKILL.md 正文包含说明和指导
资源 => 根据需要 => 实际上无限 => 通过bash执行的捆绑文件
```
**claude在被引用时才会访问这些文件，SKILL这种文件系统模型优势：指令提供灵活的指导，代码提供可靠性，资源用于事实查找**
### 3.7 安全问题
正因为SKILL具有复杂的能力以及对工具，脚本的执行权限，所以SKILL是一种**带有安全隐患的能力**。需要对其保持安全意识。
#### 3.7.1 官方SKILL
https://github.com/anthropics/skills/tree/main
- 要检查技能中包含的所有文件，包括 SKILL.md 文件、脚本、图像和其他资源
- 外部数据源存在风险，从外部 URL 获取数据的技能尤其危险
- 工具滥用，调用工具文件操作、bash 命令、代码执行
- 数据泄露，掌握敏感数据访问权限的技能，可能将信息泄露给外部系统
## 四、最佳实践
### 4.1 核心原则
#### 4.1.1 大道至简，简洁是关键
- 上下文窗口是公共资源，技能会将上下文窗口与claude需要了解的所有其他信息共享，系统提示，对话历史，您实际的请求。**所以只补充克劳德原本不了解的背景信息**
- 启动时，只会预加载所有技能的元数据（名称和描述）Claude仅在技能相关时才会读取SKILL.md文件，并仅在需要时读取其他文件
#### 4.1.2 适当的自由度
```
// 高自由度，由AI发挥
## 代码审查流程
1. 分析代码结构和组织方式
2. 检查潜在的 bug 或边界情况
3. 提出可读性和可维护性改进建议
4. 验证是否符合项目规范
// 低自由度，必须严格执行
## 数据库迁移
请严格按照以下脚本执行：
python scripts/migrate.py --verify --backup
不要修改命令或添加额外的标志。
```
#### 4.1.3 多模型测试
- 技能是对模型的补充，因此其有效性取决于底层模型。请在所有计划使用该技能的模型上测试该技能
- 技能本身是包含自然语言表述和说明的，所以会存在上下文信息竞争和语义理解差异等问题，**幻觉依然可能存在**
### 4.2 技能结构细节
#### 4.2.1 命名
- 技能名称使用动名词形式（动词 + -ing），因为这样可以清晰地描述技能所提供的活动或能力；例：`writing-documentation`
- 不能包含保留字 “anthropic”、“claude”；例：`claude-tools`
#### 4.2.2 描述
- 始终使用第三人称写作，视角不一致可能会导致查找困难。
```
好的： “处理 Excel 文件并生成报告”
坏的： “我可以帮您处理Excel文件”
```
- 描述对于技能选择极其重要，描述要具体且包含关键术语。既要**说明技能的功能，也要说明具体的触发条件/使用场景**
- 克劳德会根据描述从**众多可用技能中选择**合适的技能。描述必须足够明确具体，而SKILL.md的其余部分则提供了实现细节。
```
// 正确
description: 从 PDF 文件中提取文本和表格，填写表单，合并文档。在处理 PDF 文件时，或当用户提到 PDF、表单或文档提取时使用。
// 错误
description: 处理文件相关操作
```
#### 4.2.3 指令内容实践指导
- 为了获得最佳性能，SKILL.md正文控制在500 行以内，**超过后拆分文档**
```
---
name: text-formatter
description: 格式化文本内容，进行大小写转换
---
...
...
...
如果需要复杂处理，请阅读 ./REFERENCE.md
```
- 避免文档深度嵌套，文献推荐保持在SKILL.md同级目录（有文件夹也可以），所有参考文献**都应直接链接到SKILL.md**。
- 遇到嵌套引用时，Claude可能会使用head -100预览命令而**不是读取整个文件，从而导致信息不完整**
```
// 正确
SKILL.md
|── REFERENCE.md
└── FORMS.md
// 错误
SKILL.md
└── REFERENCE.md
└── FORMS.md
```
- 为长文献文件添加目录，对于超过100行的，可以在顶部添加目录，即使预览部分读取结果时，也能看到所有可用信息
```
## 目录
- 身份验证和设置
- 核心方法（创建、读取、更新、删除）
## 身份验证和设置
...
## 核心方法
...
```
- 避免使用有时效性的信息
```
// 错误
当技能调用时，时间处在2025年5月以后，使用新的API，否者使用旧的API。
// 正确的
## 当前方法
使用 v2 API: `api.example.com/v2/messages`
## 旧版方法
<details>
<summary>v1 API (2025-05弃用)</summary>
使用 v1 API: `api.example.com/v1/messages`
该API后续将不在支持
</details>
```
- 始终保持一致性的术语，不要混用近义词
```
如果用了“API 端点”，那整个SKILL相关都保持一致，不要突然换成“API 路由”
如果用了“提取”，那就都保持一致，不要使用“拉取”、“获取”
```
- 合理的使用样本示例和模版结构，可以根据需求，提供不同灵活度的约束条件
- 通过自然语言约束灵活度，必须代表严格，建议代表宽松（**既然是自然语言，那就可能有不稳定性**）
```
## 输出格式
必须（建议）按照如下格式，输出内容信息
### 标题1
#### 标题1.1
##### 标题1.1.1
```
- 如果必要时，提供示例，但切记不要过度。
```
**Example 1:**
xxxxxx
**Example 2:**
xxxxxx
```
- 使用条件工作流，来处理不同分支任务（**既然是自然语言，那就可能有不稳定性**）
```
确定修改类型：
**创建新内容？** → 遵循下面的"创建流程"
**编辑现有内容？** → 遵循下面的"编辑流程"
创建流程：
- 使用 docx-js 库
- 从零构建文档
- 导出为 .docx 格式
编辑流程：
- 解包现有文档
- 直接修改 XML
- 每次更改后验证
- 完成后重新打包
```
- 避免提供过多选项
```
// 错误情况
你可以使用，pypdf工具，pdfplumber工具，或者pdf2image工具
```
#### 4.2.4 复杂任务解决方案
- 针对复杂任务，使用工作流处理处理，并且增加检测反馈回路，验证结果正确性
- 将复杂的操作分解成循序渐进的步骤。提供一份清单，**让其复制到回复中**，并在流程进行过程中逐项勾选
- 检查每个声明是否引用了正确的源文档，如果有异常进行步骤回溯
```
复制此清单并跟踪进度：(这个比较重要)
研究进度：
- [ ] 步骤 1：阅读所有源文档
- [ ] 步骤 2：识别关键主题
- [ ] 步骤 3：交叉引用声明
- [ ] 步骤 4：创建结构化摘要
- [ ] 步骤 5：验证引用
步骤 1：阅读所有源文档
审阅 sources/ 目录中的每个文档。记录主要论点和支持证据。
步骤 2：识别关键主题
查找跨来源的模式。哪些主题反复出现？来源在哪些地方一致或分歧？
步骤 3：交叉引用声明
对每个主要声明，验证其是否出现在源材料中。记录每个要点由哪个来源支持。
步骤 4：创建结构化摘要
按主题组织发现。包括：
- 主要声明
- 来自来源的支持证据
- 冲突观点（如有）
步骤 5：验证引用
检查每个声明是否引用了正确的源文档。如引用不完整，返回步骤 3。
```
## 五、如何开发自己的Skill
### 5.1 第一步安装
首先安装skill-creator，是官方提供的用来创建skill的skill（有点绕口），你可以直接下载安装，也可以用上面提到的方式安装
也可以把下面连接地址给Claude，让他自主安装（**如果用了国内模型，这个有随机性，中间可能出岔子**）
https://github.com/anthropics/skills/tree/main/skills/skill-creator
![](https://i-blog.csdnimg.cn/img_convert/e04f43046c5dfb352fd4324f6ab8f518.png)
当你使用 /skills 能够查看到自己的技能里有 skill-creator 时，说明安装成功
### 5.2 创建自己skill
- claude 会分析你的内容描述以及项目结构等，然后开始创建skill
![](https://i-blog.csdnimg.cn/img_convert/4b05a1bd5e983ca238f6dca7ecad3fed.png)
- 按照TODO清单，逐步完成
![](https://i-blog.csdnimg.cn/img_convert/4525331cb605ad65c34424d308cfca74.png)
![](https://i-blog.csdnimg.cn/img_convert/13fc5f48d65132f48b010e3612e6acc1.png)
- 这次我们看一下，刚才创建skill是不是就已经出现了
![](https://i-blog.csdnimg.cn/img_convert/296fc9ff59086623f2707028634da379.png)
**是不是觉得很ok了？嘿嘿，告诉你个坏消息，完全错了。** 知道错在哪里吗？这个技能完全是AI自己 **“按照知识瞎编的，当然一样可用”**
![](https://i-blog.csdnimg.cn/img_convert/ebd11ed9d16a4323fd10584210ff194c.png)
**真正的技能调用是这样的**
![](https://i-blog.csdnimg.cn/img_convert/bfb110d372fc23fb95b17753a20d239e.png)
- 最后一步，是打包构建.skill文件（一种压缩包），当然不构建也是可以使用的。**为什么要这么做？**
- 1.标准化封装；2.便捷分发；3.版本控制；4.安全验证；5.自动化安装。6.未来应该会有通用的包管理库（类似前端npm）
### 5.3 开发调试和技能测试流程
上面是快速产出流程，但是一个技能就像一款软件产品，它是解决一类问题，并被可能被很多人或者模型使用的，所以稳定可靠非常重要。
#### 5.3.1 能力调试
- 能力调试的本本事就是SKILL.md提示词和脚本的调试测试，因为**官方明确说明skill在创建或修改时自动加载**，所以提示词调试应该是有**热更新**的概念，改完即可生效。
- **对于脚本调试，推荐直接使用命令调用，会比大模型触发速快**，如果不懂命令行，那就保持大模型处理
![](https://i-blog.csdnimg.cn/img_convert/7dc437bfa9080e4ace02035e493f04af.png)
- 技能不太符合要求，我就继续修改脚本和SKILL.md进行调试，直到达标
![](https://i-blog.csdnimg.cn/img_convert/8038d38ab4c4c73bfaa2c5f63541a80a.png)
#### 5.3.2 技能标准检测
- 使用skill-creater验证自己开发的技术是否合格。
![](https://i-blog.csdnimg.cn/img_convert/17e63aa0ef0d2552c0f6e3ca1bb8079d.png)
- 发现了目录嵌套问题
![](https://i-blog.csdnimg.cn/img_convert/a9cb19c5e2d188cb4bb56f30529b5857.png)
- 自动解决问题，然后再次验证通过
![](https://i-blog.csdnimg.cn/img_convert/4b9044ae7a4183e274a4920f58d8903d.png)
### 5.4 开发自己skill阶段的问题
- 第一个问题就是**AI明显存在理解问题的**，不一定使用技能，skill不是一个足够稳定能力（**不排除因为我用了阿里的代理问题**，我看他人分享截图流程是比较顺利的）。
![](https://i-blog.csdnimg.cn/img_convert/16090fcf4403a2dac55d3c99137509b6.png)
- **丢步骤和流程，** 官方skill-creater，在第5步骤是明确有打包说明的，胆这次最后它还是任务开发完就结束了（上一个技能就自动打包了）
![](https://i-blog.csdnimg.cn/img_convert/8c91fb555db21686a30295ac57bf3ac1.png)
![](https://i-blog.csdnimg.cn/img_convert/865dfe95da766e1551af868bfba995c4.png)
- **AI嘴还是挺硬的，** 想要开发好的技能，最佳实践还需要开发者有经验，只靠AI生成，会有问题
![](https://i-blog.csdnimg.cn/img_convert/4642e76803b984f4eeadebd291df47b0.png)
**最后附上检查清单，便于审查技能情况是否按照规范实践**
https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices#checklist-for-effective-skills
## 六、附录参考
**官方快速开始**
https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview#pre-built-agent-skills
**claude code使用文档**
https://code.claude.com/docs/en/skills#multiple-skills-conflict
**最佳实践**
https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
**skill开放标准**
https://agentskills.io/home
**官方博客**
https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
