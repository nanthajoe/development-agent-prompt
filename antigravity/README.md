# 🤖 Antigravity Agentic Assembly Line (AAL)
> A deterministic, human-in-the-loop multi-agent development pipeline for Google Antigravity IDE.

This workspace utilizes a strict, modular multi-agent pipeline designed to handle software engineering tasks with high precision, absolute architectural traceability, and ironclad sandbox safety. 

By separating file modification from command execution, this framework eliminates autonomous code corruption, maintains a pristine Git history, and enforces automated, chronologically sorted release tracking.

---

## 🛠️ The Core Agent Assembly Line

The pipeline splits responsibilities across 7 specialized, single-purpose agents:
```mermaid
flowchart TD
    A(["👤 User Request"])

    A --> AA

    AA["💬 **0. Discussion Agent**\nCaptures requirements in docs/discussions/"]
    AA --> B

    B["🗺️ **1. Planning Agent**\nCreates docs/plans/v0.0 - ..."]
    B --> C

    C["⚙️ **2. Implementation Agent**\nModifies code files ONLY\n_(Sandbox: No execution)_"]
    C --> D & E

    D["🧪 Manual Testing"]
    E["🐛 **3. Bug Fixing Agent**\nSurgical Hotfixes\n_(Hypothesis First)_"]

    D --> F
    E --> F

    F["📝 **4. Documentation Agent**\nCreates docs/changelogs/[TS] - ..."]
    F --> G

    G["✅ **5. Commit Agent**\nRecommends Conventional Commits"]

    style A fill:#6366f1,color:#fff,stroke:none
    style AA fill:#06b6d4,color:#fff,stroke:none
    style B fill:#0ea5e9,color:#fff,stroke:none
    style C fill:#0ea5e9,color:#fff,stroke:none
    style D fill:#64748b,color:#fff,stroke:none
    style E fill:#f59e0b,color:#fff,stroke:none
    style F fill:#8b5cf6,color:#fff,stroke:none
    style G fill:#10b981,color:#fff,stroke:none
```

| Agent Name | Trigger Command | Allowed Tools | Core Deliverable | Primary Boundary |
| :--- | :--- | :--- | :--- | :--- |
| **.gitignore Agent** | `/.gitignore Agent` | `read_file`, `view_directory` | Copyable `.gitignore` block | Read-only; never writes to `.gitignore` directly |
| **Discussion Agent** | `/Discussion Agent` | `write_file`, `view_directory` | `docs/discussions/[TS] - *.md` | Forbidden from writing code or plan files |
| **Planning Agent** | `/Planning Agent` | `read_file`, `write_file`, `Google Search` | `docs/plans/v0.0 - *.md` | Forbidden from altering source code |
| **Implementation Agent** | `/Implementation Agent` | `read_file`, `write_file`, `view_directory` | Functional Code Architecture | Forbidden from executing terminal commands |
| **Bug Fixing Agent** | `/Bug Fixing Agent` | `read_file`, `write_file`, `Google Search` | Surgical Code Hotfixes | Forbidden from editing without explicit chat approval |
| **Documentation Agent** | `/Documentation Agent` | `read_file`, `write_file`, `view_directory` | `docs/changelogs/[TS] - *.md` | Forbidden from altering application logic |
| **Commit Agent** | `/Commit Agent` | `read_file`, `view_directory` | Copy-pasteable Git command | Read-only; cannot commit or push automatically |

---

## 📖 Step-by-Step Execution Guide

### 📂 Preparation: Project Directory Structure
Before running your first cycle, ensure your workspace includes these tracking directories:
```bash
mkdir -p docs/discussions docs/plans docs/changelogs .agents/workflows
```

---

### 🟫 Step 0: Protect Your Repository with the .gitignore Agent

Before starting any new project or feature cycle, run the **.gitignore Agent** to ensure secrets, build artifacts, and IDE clutter are never accidentally committed.

* **How to invoke — Generate (new project):**
```text
/.gitignore Agent mode: "generate"
```

* **How to invoke — Check (existing project):**
```text
/.gitignore Agent mode: "check"
```

* **Expected Result:** The agent scans your workspace for tech stack marker files (`package.json`, `requirements.txt`, `*.xcodeproj`, etc.) and IDE artifacts (`.vscode/`, `.idea/`). It outputs a complete, ready-to-paste `.gitignore` block or an audit report of missing entries — entirely offline. You then paste the result into your `.gitignore` manually.

---

### 🟩 Step 1: Clarifying Requirements with the Discussion Agent

Before writing any plan, invoke the **Discussion Agent** with your raw feature idea. It will interview you with structured questions and produce a `docs/discussions/` document — the **token firewall** that prevents ambiguous inputs from reaching the Planning Agent.

* **How to invoke:**
```text
/Discussion Agent topic: "We need an API endpoint at /health that checks server status."
```

* **Expected Result:** The agent asks 6 clarifying questions in one message (Goal, Scope, Non-Goals, Constraints, Edge Cases, Success Criteria). After your answers and a confirmation, it saves a structured document to `docs/discussions/[YYYYMMDD-HHMM] - [topic].md` and prints the exact command to pass it to the Planning Agent.

---

### 🟦 Step 2: Architecting with the Planning Agent

Once the discussion document is saved, feed it directly to the **Planning Agent** to produce a concrete implementation blueprint.

* **How to invoke:**
```text
/Planning Agent discussion_path: "docs/discussions/20260628-1106 - health-check-endpoint.md"
```

* **Expected Result:** A new engineering blueprint is compiled under `docs/plans/vX.X - [feature-slug] - [phase if any].md` broken down into *Context, Architecture, Affected Components, Steps,* and *Verification*.

---

### 🟪 Step 3: Coding with the Implementation Agent

Once the blueprint is generated, feed it directly to the **Implementation Agent**. This agent operates in a closed loop, writing production code without executing scripts.

* **How to invoke:**
```text
/Implementation Agent plan_path: "docs/plans/v0.1 - health-check-endpoint.md"
```


* **Expected Result:** The agent systematically applies the structural alterations across your source tree. If the installation of dependencies or migrations is required, it will cleanly output a code block instructions for you to run manually.

---

### 🟨 Step 4: Resolving Quirks with the Bug Fixing Agent

If verification testing fails or an unexpected logical edge-case appears, deploy the **Bug Fixing Agent**.

* **How to invoke:**
```text
/Bug Fixing Agent bug_description: "The /health route is returning undefined for the environment mode, but there are no crash logs in the terminal."
```


* **Expected Protocol:** The agent will perform behavioral tracking, state its hypothesis, and output a strict *Blast Radius Assessment*. It will explicitly prompt you for approval. Type **"Go ahead"** to apply the surgical fix.

---

### 🟫 Step 5: Tracking with the Documentation Agent

With features implemented and verified, run the **Documentation Agent** to capture the historical footprint.

* **How to invoke:**
```text
/Documentation Agent input_path: "docs/plans/v0.1 - health-check-endpoint.md"
```


*(For tracking bug hotfixes, substitute the input path with your bug trace report or file context).*
* **Expected Result:** A timestamped, separate change file is pushed to your logs directory following a strict chronological layout:
`docs/changelogs/[YYYYMMDD-HHMM] - vX.X - [implementation/patch] - [description].md`
* It will automatically align environment variations inside your root `.env.example`.

---

### ⬛ Step 6: Finalizing with the Commit Agent

When the workspace state is clean and documentation is synchronized, call the **Commit Agent** to lock your historical timeline down.

* **How to invoke:**
1. Stage your working changes in your terminal or IDE panel:
```bash
git add .
```
2. In the chat input, trigger:
```text
/Commit Agent
```

* **Expected Result:** The agent scans your file diffs and documentation timestamp fragments, outputting three clean, standardized **Conventional Commit** variants. Simply click the copy button on your preferred variant, paste it into your terminal shell, and hit enter.

---

## 🔒 Safety and Operational Principles

1. **Human-In-The-Loop Control:** Agents act as co-pilots, not pilots. Code execution and git persistence layer write operations are intentionally isolated to your control.
2. **Atomic Context Isolation:** By loading dedicated agents with dedicated skill definitions, your context window remains clean, completely eliminating hallucinations born from overflowing system prompts.
3. **No Code Placeholders:** Every coding layer agent is strictly prohibited from deploying short-hands like `// TODO: implement later` to preserve production continuity.