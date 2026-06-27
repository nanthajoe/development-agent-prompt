# 🤖 Agentic Assembly Line (AAL) — Claude Code Edition
> A deterministic, human-in-the-loop multi-agent development pipeline for **Claude Code** CLI.

This workspace utilizes a strict, modular multi-agent pipeline designed to handle software engineering tasks with high precision, absolute architectural traceability, and ironclad sandbox safety.

By separating file modification from command execution, this framework eliminates autonomous code corruption, maintains a pristine Git history, and enforces automated, chronologically sorted release tracking.

---

## 🛠️ The Core Agent Assembly Line

The pipeline splits responsibilities across 5 specialized, single-purpose agents:
```mermaid
flowchart TD
    A(["👤 User Request"])

    A --> B

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
    style B fill:#0ea5e9,color:#fff,stroke:none
    style C fill:#0ea5e9,color:#fff,stroke:none
    style D fill:#64748b,color:#fff,stroke:none
    style E fill:#f59e0b,color:#fff,stroke:none
    style F fill:#8b5cf6,color:#fff,stroke:none
    style G fill:#10b981,color:#fff,stroke:none
```

| Agent Name | Slash Command | Agent File | Core Deliverable | Primary Boundary |
| :--- | :--- | :--- | :--- | :--- |
| **Planning Agent** | `/planning-agent` | [planning-agent.md](./planning-agent.md) | `docs/plans/v0.0 - *.md` | Forbidden from altering source code |
| **Implementation Agent** | `/implementation-agent` | [implementation-agent.md](./implementation-agent.md) | Functional Code Architecture | Forbidden from executing install/build commands |
| **Bug Fixing Agent** | `/bug-fixing-agent` | [bug-fixing-agent.md](./bug-fixing-agent.md) | Surgical Code Hotfixes | Forbidden from editing without explicit chat approval |
| **Documentation Agent** | `/documentation-agent` | [documentation-agent.md](./documentation-agent.md) | `docs/changelogs/[TS] - *.md` | Forbidden from altering application logic |
| **Commit Agent** | `/commit-agent` | [commit-agent.md](./commit-agent.md) | Copy-pasteable Git command | Read-only; cannot commit or push automatically |

---

## 📖 Step-by-Step Execution Guide

### 📂 Preparation: Project Directory Structure
Before running your first cycle, ensure your workspace includes these tracking directories and copy the agent command files into `.claude/commands/`:
```bash
mkdir -p docs/plans docs/changelogs .claude/commands
cp claude-code/*.md .claude/commands/
```

---

### 🟩 Step 1: Architecting with the Planning Agent

When introducing a new feature or structural overhaul, invoke the **Planning Agent** to translate concepts into a concrete execution plan.

* **How to invoke:**
```text
/planning-agent "We need an API endpoint at /health. It should check if the server is up and return a JSON payload with status:'OK' and a timestamp. It must read an environment variable APP_ENVIRONMENT."
```

* **Expected Result:** A new engineering blueprint is compiled under `docs/plans/v0.0 - [feature description].md` broken down into *Context, Architecture, Affected Components, Steps,* and *Verification*.

---

### 🟦 Step 2: Coding with the Implementation Agent

Once the blueprint is generated, feed it directly to the **Implementation Agent**. This agent operates in a closed loop, writing production code without executing scripts.

* **How to invoke:**
```text
/implementation-agent "docs/plans/v0.0 - health check endpoint.md"
```

* **Expected Result:** The agent systematically applies the structural alterations across your source tree. If the installation of dependencies or migrations is required, it will cleanly output a code block of instructions for you to run manually.

---

### 🟨 Step 3: Resolving Quirks with the Bug Fixing Agent

If verification testing fails or an unexpected logical edge-case appears, deploy the **Bug Fixing Agent**.

* **How to invoke:**
```text
/bug-fixing-agent "The /health route is returning undefined for the environment mode, but there are no crash logs in the terminal."
```

* **Expected Protocol:** The agent will perform behavioral tracking, state its hypothesis, and output a strict *Blast Radius Assessment*. It will explicitly prompt you for approval. Type **"Go ahead"** to apply the surgical fix.

---

### 🟪 Step 4: Tracking with the Documentation Agent

With features implemented and verified, run the **Documentation Agent** to capture the historical footprint.

* **How to invoke:**
```text
/documentation-agent "docs/plans/v0.0 - health check endpoint.md"
```

*(For tracking bug hotfixes, substitute the input path with your bug trace report or file context).*
* **Expected Result:** A timestamped, separate change file is pushed to your logs directory following a strict chronological layout:
`docs/changelogs/[YYYYMMDD-HHMM] - v0.0 - [implementation/patch] - [description].md`
* It will automatically align environment variations inside your root `.env.example`.

> 💡 The Documentation Agent uses `date +"%Y%m%d-%H%M"` internally via Bash to auto-generate the accurate timestamp — no manual input needed.

---

### ⬛ Step 5: Finalizing with the Commit Agent

When the workspace state is clean and documentation is synchronized, call the **Commit Agent** to lock your historical timeline down.

* **How to invoke:**
1. Stage your working changes in your terminal:
```bash
git add .
```
2. In the Claude Code chat input, trigger:
```text
/commit-agent
```

* **Expected Result:** The agent scans your file diffs and documentation timestamp fragments, outputting three clean, standardized **Conventional Commit** variants. Simply click the copy button on your preferred variant, paste it into your terminal shell, and hit enter.

---

## 🔒 Safety and Operational Principles

1. **Human-In-The-Loop Control:** Agents act as co-pilots, not pilots. Code execution and git persistence layer write operations are intentionally isolated to your control.
2. **Bash Safety Boundary:** Claude Code has native Bash access — each agent file explicitly constrains Bash usage to **read-only** operations (`grep`, `cat`, `git diff --staged`, `date`). Write and execute operations always require explicit human approval.
3. **No Code Placeholders:** Every coding layer agent is strictly prohibited from deploying short-hands like `// TODO: implement later` to preserve production continuity.
