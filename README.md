<div align="center">

# Codex Content Production Kit

Reusable prompts, workflows, templates, and agent rules for producing structured technical learning documents with Codex or an IDE chat model.

<a href="#english">English</a>
&nbsp;&nbsp;|&nbsp;&nbsp;
<a href="#中文说明">中文</a>

</div>

---

<a id="english"></a>

## What is this?

This repository is a small, file-based content production system for AI-assisted technical writing.

It helps you turn a topic into a structured document by separating:

- persistent project rules;
- task-specific prompts;
- source and evidence notes;
- page or section planning;
- generation and review workflows;
- reusable input and QA templates.

The original content focus is machine-learning interview preparation and Xiaohongshu-style 5–6 page notes. The same workflow can also produce other technical documents. The included [Java OOP guide](examples/java-oop-guide.md) is a working example generated with this approach.

## What can it produce?

Depending on the files you provide, the kit can produce:

- a topic shortlist and episode plan;
- a source-normalized brief with cautious provenance wording;
- a 5–6 page content plan;
- a complete technical lecture note;
- interview answer templates, examples, pitfalls, and follow-up questions;
- LaTeX-ready body content that preserves an existing template;
- a final QA summary.

It is a content workflow, not an automatic publishing platform. It does not log in to Xiaohongshu, publish posts, or replace your own source verification.

## Repository structure

```text
.
├── AGENTS.md                 # Persistent project-level instructions
├── HANDOFF.md                # Context to carry between sessions
├── prompts/                  # Reusable task prompts
├── workflows/                # Multi-step production and review processes
├── templates/                # Structured briefs, source logs, plans, and QA
├── docs/                     # Design and usage documentation
├── examples/                 # Example inputs and generated documents
└── starter/                  # Minimal files to copy into another project
```

Important files:

- [AGENTS.md](AGENTS.md): rules that should remain active throughout a project.
- [prompts/00-master-system-prompt.md](prompts/00-master-system-prompt.md): general content-generation contract.
- [prompts/02-episode-generator.md](prompts/02-episode-generator.md): structured episode/document generation.
- [prompts/03-source-normalizer.md](prompts/03-source-normalizer.md): source classification and cautious wording.
- [prompts/04-latex-polisher.md](prompts/04-latex-polisher.md): language and layout-density refinement.
- [workflows/episode-production-workflow.md](workflows/episode-production-workflow.md): recommended production order.
- [templates/qa-checklist.md](templates/qa-checklist.md): final acceptance checklist.
- [examples/java-oop-guide.md](examples/java-oop-guide.md): generated Java OOP lecture-note example.

## Use it with Codex

### Option A: Use the repository as your working project

1. Clone or download this repository.
2. Open the repository folder in Codex.
3. Keep [AGENTS.md](AGENTS.md) at the project root so Codex can read the persistent rules.
4. Tell Codex what you want to produce, for example:

   ```text
   Read AGENTS.md, templates, and the relevant prompts.
   Create a Chinese lecture note about Java object-oriented programming.
   Cover encapsulation, inheritance, polymorphism, abstraction, interfaces,
   composition, and the main Java-specific rules.
   Save the result under examples/java-oop-guide.md.
   Do not convert it to LaTeX yet.
   ```

5. Ask Codex to review the output against [templates/qa-checklist.md](templates/qa-checklist.md).

### Option B: Copy the starter configuration into another project

The [starter/](starter/) folder contains a smaller setup:

1. Copy `starter/AGENTS.md` to the root of your target project.
2. Copy `starter/.codex/` to the target project.
3. Adjust the rules if your topic is not machine-learning interview content.
4. In Codex, ask it to read the project rules and the task file before writing.

For a new document, prepare these files first:

- [templates/episode-brief.md](templates/episode-brief.md)
- [templates/source-log.md](templates/source-log.md)
- [templates/page-plan.md](templates/page-plan.md)

Then run the workflow in this order:

```text
topic selection
  -> source normalization
  -> brief and page plan
  -> document generation
  -> language/layout polishing
  -> QA review
```

## Use it with an IDE chat model

This repository also works with ChatGPT, GitHub Copilot Chat, Claude, or another IDE-integrated chat model that can read workspace files.

### Workspace workflow

1. Open the repository or your target project as the IDE workspace.
2. Add the relevant files to the chat context:
   - `AGENTS.md`;
   - one task prompt from `prompts/`;
   - the relevant files in `templates/`;
   - any source notes or the existing document to edit.
3. Give the model a bounded request:

   ```text
   Read the attached project rules and templates.
   Produce a Chinese Java OOP lecture note, not a social-media post.
   Keep the scope to OOP and Java-specific object-model rules.
   Include runnable Java examples where useful.
   Write the result to examples/java-oop-guide.md.
   ```

4. Review the diff in the IDE.
5. Ask for a separate QA pass instead of accepting the first draft:

   ```text
   Review the generated document against the project rules.
   Check conceptual coverage, Java accuracy, scope, examples, and structure.
   Report issues first; do not rewrite unrelated files.
   ```

The exact command or UI differs between IDEs. The important part is to provide the model with the rules, the task prompt, the structured templates, and a precise output path.

## Adapting the kit to a different subject

The repository currently contains ML-interview-oriented defaults. To use it for Java, databases, networking, or another subject:

1. Keep the workflow and QA structure.
2. Update the project-level [AGENTS.md](AGENTS.md) so its mission matches the new subject.
3. Replace ML-specific source requirements with the evidence standards appropriate to your subject.
4. Reuse the generation and polishing prompts, but state the new output format.
5. Put generated documents under `examples/` or a subject-specific directory.

Do not silently treat an example document as the new project-wide rule. The Java OOP file is an example; the repository defaults remain ML-focused until you intentionally change them.

## Design principles

- Keep persistent instructions short and high-signal.
- Separate planning, sourcing, generation, polishing, and review.
- Make source confidence explicit.
- Preserve the user's existing document or LaTeX structure.
- Prefer a narrow, useful topic over an encyclopedic dump.
- Use a second QA pass to catch scope, accuracy, and formatting problems.

## Limitations

- The model can still make factual mistakes; verify technical claims and sources.
- The repository does not include your private LaTeX template.
- GitHub rendering supports the language links above as navigation buttons, not a JavaScript toggle. They jump between the English and Chinese sections.
- Automatic publishing, image generation, and account automation are outside this repository.

## Publish this project to GitHub

The folder is GitHub-ready but is not initialized as a Git repository yet. From the project folder, run:

```bash
git init
git add .
git commit -m "Initial content production kit"
git branch -M main
git remote add origin https://github.com/<your-account>/<your-repository>.git
git push -u origin main
```

Create the empty GitHub repository before running the final `git remote` and `git push` commands. Do not commit private source notes, credentials, generated session state, or an unpublished LaTeX template.

## License

No license has been selected yet. Add a license before accepting external contributions or distributing the repository as reusable software.

---

<a id="中文说明"></a>

## 中文说明

这是一个给 Codex、GitHub Copilot Chat、ChatGPT、Claude 等 IDE 聊天模型使用的**技术内容生产模板项目**。

它把“让模型写一份内容”拆成几个稳定步骤：

- 项目长期规则；
- 任务提示词；
- 来源和证据记录；
- 分页或章节规划；
- 生成和复核流程；
- 结构化模板与 QA 清单。

项目最初面向“机器学习面试题 + 小红书 5–6 页图文”，但流程也可以迁移到 Java、数据库、计算机网络等主题。当前的 [Java OOP 讲义示例](examples/java-oop-guide.md) 就是使用这套方法生成的。

### 能产出什么？

它可以帮助模型产出：

- 选题列表和内容规划；
- 来源分类和谨慎表述；
- 5–6 页分页方案；
- 完整技术讲义；
- 面试回答模板、代码示例、易错点和追问；
- 可以插入既有 LaTeX 模板的正文；
- 最终 QA 检查结果。

它不是自动发布软件，不会自动登录小红书、发布内容，也不能代替人工核验技术资料。

### 在 Codex 中使用

#### 方式一：直接把本仓库作为工作项目

1. 克隆或下载本仓库。
2. 在 Codex 中打开项目目录。
3. 保留根目录的 [AGENTS.md](AGENTS.md)，让 Codex 读取长期规则。
4. 直接告诉 Codex 目标、范围和输出路径，例如：

   ```text
   请先阅读 AGENTS.md、templates 和相关 prompts。
   生成一份中文 Java 面向对象编程讲义。
   覆盖封装、继承、多态、抽象、接口、组合以及主要 Java 规则。
   保存到 examples/java-oop-guide.md。
   先不要转换成 LaTeX。
   ```

5. 再让 Codex 按 [templates/qa-checklist.md](templates/qa-checklist.md) 做一次检查。

#### 方式二：复制 starter 配置到其他项目

将 [starter/](starter/) 中的内容复制到你的目标项目：

1. 复制 `starter/AGENTS.md` 到目标项目根目录。
2. 复制 `starter/.codex/` 到目标项目。
3. 如果主题不是机器学习面试，修改其中的主题规则。
4. 在 Codex 中要求它先读取规则和任务文件，再开始写作。

### 在 IDE 聊天大模型中使用

这套文件也可以用于 VS Code、JetBrains 等 IDE 里的 Copilot Chat、ChatGPT、Claude 或其他能读取工作区文件的模型。

使用步骤：

1. 在 IDE 中打开本仓库或你的目标项目。
2. 把以下文件加入聊天上下文：
   - `AGENTS.md`；
   - `prompts/` 中相关的任务提示词；
   - `templates/` 中相关模板；
   - 你的资料、已有文档或需要修改的文件。
3. 给模型一个明确、有限的任务，例如：

   ```text
   请阅读已提供的项目规则和模板。
   生成一份中文 Java OOP 讲义，不要写成小红书短文。
   范围限定为 OOP 和 Java 对象模型相关规则。
   必要时加入可运行的 Java 示例。
   将结果写入 examples/java-oop-guide.md。
   ```

4. 在 IDE 里检查文件 diff。
5. 再单独要求模型做 QA，不要直接相信第一版：

   ```text
   请根据项目规则复核刚才的讲义。
   检查知识覆盖、Java 准确性、范围、代码示例和结构。
   先报告问题，不要修改无关文件。
   ```

### 迁移到其他主题

当前根目录规则仍然是机器学习内容生产规则。要正式用于 Java：

1. 保留工作流和 QA 结构；
2. 修改根目录 [AGENTS.md](AGENTS.md) 的任务目标；
3. 将机器学习来源要求替换为 Java 技术资料的核验标准；
4. 继续复用生成、精修和检查提示词；
5. 将输出放到 `examples/` 或新的主题目录。

注意：Java OOP 讲义是示例，不会自动改变整个项目的默认方向。

### 语言按钮说明

README 顶部的 `English` 和 `中文` 是 GitHub 兼容的页面跳转按钮。GitHub README 不执行 JavaScript，因此它们不是动态下拉切换，而是跳转到对应语言区域。

---

## Suggested next steps

- Add your own LaTeX template under `assets/latex/` or `templates/latex/`.
- Customize [AGENTS.md](AGENTS.md) for your real subject.
- Add a project license before publishing for reuse.
- Keep generated documents and source notes separate from permanent rules.
