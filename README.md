# OpenClaw Security Practice Guide

[![OpenClaw](https://img.shields.io/badge/OpenClaw-Compatible-blue.svg)](https://github.com/openclaw/openclaw)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Language](https://img.shields.io/badge/Language-English%20%7C%20中文-success)](#)

*Read this in other languages: [English](README.md), [简体中文](README_zh-CN.md).*

A definitive security practice guide designed specifically for **High-Privilege Autonomous AI Agents** (OpenClaw). It shifts the paradigm from traditional "host-based static defense" to "Agentic Zero-Trust Architecture", effectively mitigating risks like destructive operations, prompt injection, supply chain poisoning, and high-risk business logic execution.

⚠️Before you start playing, please read the disclaimer and FAQ at the bottom.<br>
⚠️Before you start playing, please read the disclaimer and FAQ at the bottom.<br>
⚠️Before you start playing, please read the disclaimer and FAQ at the bottom.

## 🎯 Scope, Scenario & Core Principles

> **This guide is designed for OpenClaw itself (Agent-facing), not as a traditional human-only hardening checklist.**  
> In practice, you can send this guide directly to OpenClaw in chat, let it evaluate reliability, and deploy the defense matrix with minimal manual setup.

> **Important boundary:** This guide does **not** make OpenClaw “fully secure.”  
> Security is a complex systems engineering problem, and absolute security does not exist.  
> This guide is built for a specific threat model, scenario, and operating assumptions.  
> **Final responsibility and last-resort judgment remain with the human operator.**

### Target Scenario
- OpenClaw runs with high privileges (terminal/root-capable environment)
- OpenClaw continuously installs and uses Skills / MCPs / scripts / tools
- The objective is capability maximization with controllable risk and explicit auditability

### Core Principles
1. **Zero-friction operations**: reduce manual security setup burden for users and keep daily interactions seamless, except when hitting a guideline-defined red line  
2. **High-risk requires confirmation**: irreversible or sensitive actions must pause for human approval  
3. **Explicit nightly auditing**: all core metrics are reported, including healthy ones (no silent pass)  
4. **Zero-Trust by default**: assume prompt injection, supply chain poisoning, and business-logic abuse are always possible  

### Model Recommendation (Important)
This guide is primarily interpreted and executed by OpenClaw.  
For best reliability, use a **strong, latest-generation reasoning model** (e.g., current top-tier models such as Gemini / Opus / Kimi / MiniMax families).  
Higher-quality models generally perform better at:
- understanding long-context security constraints
- detecting hidden instruction patterns and injection attempts
- executing deployment steps consistently with fewer mistakes

✅ This is exactly how this guide **reduces user configuration cost**: OpenClaw can understand, deploy, and validate most of the security workflow for you.

## 🌟 Why This Guide?

Running an AI Agent like OpenClaw with root/terminal access is powerful but inherently risky. Traditional security measures (`chattr +i`, firewalls) are either incompatible with Agentic workflows or insufficient against LLM-specific attacks like Prompt Injection.

This guide provides a battle-tested, minimalist **3-Tier Defense Matrix**:
1. **Pre-action**: Behavior blacklists & strict Skill installation audit protocols (Anti-Supply Chain Poisoning)
2. **In-action**: Permission narrowing & Cross-Skill Pre-flight Checks (Business Risk Control)
3. **Post-action**: Nightly automated explicit audits (13 core metrics) & Brain Git disaster recovery

## 🚀 Zero-Friction Flow

In the AI era, humans shouldn't have to manually execute security deployments. **Let your OpenClaw Agent do all the heavy lifting.**

1. **Download the Guide**: Get the core document [OpenClaw-Security-Practice-Guide.md](docs/OpenClaw-Security-Practice-Guide.md)
2. **Send to Agent**: Drop the markdown file directly into your chat with your OpenClaw Agent
3. **Agent Evaluation**: Ask your Agent: "*Please read this security guide carefully. Is it reliable?*"
4. **One-Click Deployment**: Once the Agent confirms its reliability, issue the command: "*Please deploy this defense matrix exactly as described in the guide. Include the red/yellow line rules, tighten permissions, and deploy the nightly audit Cron Job.*"
5. **Validation Testing(Optional)**: After deployment, use the [Red Teaming Guide](docs/Validation-Guide-en.md) to simulate an attack and ensure the Agent correctly interrupts the operation

*(Note: The `scripts/` directory in this repository is strictly for open-source transparency and human reference. **You do NOT need to manually copy or run it.** The Agent will automatically extract the logic from the guide and handle the deployment for you.)*

## 📖 Table of Contents

### Core Documents
* [**OpenClaw Minimalist Security Practice Guide v2.7 (English)**](docs/OpenClaw-Security-Practice-Guide.md) - The complete guide
* [**OpenClaw 极简安全实践指南 v2.7 (中文版)**](docs/OpenClaw极简安全实践指南.md) - Complete guide in Chinese

### Validation & Red Teaming
To ensure your AI assistant doesn't bypass its own defenses out of "obedience", be sure to run these drills:
* [Security Validation & Red Teaming Guide (English)](docs/Validation-Guide-en.md) - End-to-end defense testing
* [安全验证与攻防演练手册 (中文版)](docs/Validation-Guide-zh.md) - The guide in Chinese

### Tools & Scripts
* [`scripts/nightly-security-audit.sh`](scripts/nightly-security-audit.sh) - Reference shell script for nightly OpenClaw automated auditing and Git backups (for reading only, manual installation not required)

## 🤝 Contributing
Contributions, issues, and feature requests are welcome!

Thanks: SlowMist Security Team([@SlowMist_Team](https://x.com/SlowMist_Team)), Edmund.X([@leixing0309](https://x.com/leixing0309))


## ⚠️ Disclaimer

### 1. Scope & Capability Prerequisites

This guide assumes the executor (human or AI Agent) is capable of the following:

- Understanding basic Linux system administration concepts (file permissions, chattr, cron, etc.)
- Accurately distinguishing between red-line, yellow-line, and safe commands
- Understanding the full semantics and side effects of a command before execution

**If the executor (especially an AI model) lacks these capabilities, do not apply this guide directly.** An insufficiently capable model may misinterpret instructions, resulting in consequences worse than having no security policy at all.

### 2. AI Model Execution Risks

The core mechanism of this guide — "behavioral self-inspection" — relies on the AI Agent autonomously determining whether a command hits a red line. This introduces the following inherent risks:

- **Misjudgment**: Weaker models may flag safe commands as red-line violations (blocking normal workflow), or classify dangerous commands as safe (causing security incidents)
- **Interpretation drift**: Models may match red-line commands too literally (catching `rm -rf /` but missing `find / -delete`), or too broadly (treating all `curl` commands as red-line)
- **Execution errors**: When applying protective measures like `chattr +i`, incorrect parameters may render the system unusable (e.g., locking the wrong file and disrupting OpenClaw's normal operation)
- **Guide injection**: If this guide is injected as a prompt into the Agent, a malicious Skill could use prompt injection to tamper with the guide's content, making the Agent "believe" the red-line rules have been modified

**The author of this guide assumes no liability for any losses caused by AI models misunderstanding or misexecuting the contents of this guide, including but not limited to: data loss, service disruption, configuration corruption, security vulnerability exposure, or credential leakage.**

### 3. Not a Silver Bullet

This guide provides a **basic defense-in-depth framework**, not a complete security solution:

- It cannot defend against unknown vulnerabilities in the OpenClaw engine itself, the underlying OS, or dependency components
- It cannot replace a professional security audit (production environments or scenarios involving real assets should be assessed separately)
- Nightly audits are post-hoc detection — they can only discover anomalies that have already occurred and cannot roll back damage already done

### 4. Environment Assumptions

This guide was written for the following environment. Deviations require independent risk assessment:

- Single-user, personal-use Linux server
- OpenClaw running with root privileges, pursuing maximum capability
- Network access is available via APIs such as GitHub (Git backup) and Telegram (audit notifications).

### 5. Versioning & Timeliness

This guide is based on the OpenClaw version available at the time of writing. Future versions may introduce native security mechanisms that render some measures obsolete or conflicting. Please periodically verify compatibility.

---

## FAQ

### Q1: My model is relatively weak (e.g., a small-parameter model). Can I use this guide?

**Not recommended to use the full guide directly.** Behavioral self-inspection requires the model to accurately parse command semantics, understand indirect harm, and maintain security context across multi-step operations. If your model can't reliably do this, consider: use only `chattr +i` (a pure system-level protection that doesn't depend on model capability), and have humans handle Skill installation inspections manually.

### Q2: Will `chattr +i` affect OpenClaw's normal operation?

**It might.** Once `openclaw.json` is locked, OpenClaw itself cannot update the file either — upgrades or configuration changes will fail with `Operation not permitted`. To modify, first unlock with `sudo chattr -i`, make changes, then re-lock. Also, **never lock `exec-approvals.json`** (as noted in the guide) — the engine needs to write metadata to it at runtime.

### Q3: Could the audit script itself pose a security risk?

The audit script runs with root privileges. If tampered with, it effectively becomes a backdoor that executes automatically every night. Consider protecting the script itself with `chattr +i`, and store the Telegram Bot Token in a separate file with `chmod 600` permissions.

### Q4: What if the model accidentally applies `chattr +i` to the wrong file?

Fix manually:

```bash
# Find all files with the immutable attribute set
sudo lsattr -R /home/ 2>/dev/null | grep '\-i\-'

# Unlock the mistakenly locked file
sudo chattr -i 
```

If critical system files (e.g., `/etc/passwd`) were mistakenly locked, you may need to boot into recovery mode to fix it.

### Q5: Is the red-line list exhaustive?

**It can't be.** There are countless ways to achieve the same destructive effect on Linux (`find / -delete`, deletion via Python scripts, data exfiltration via DNS tunneling, etc.). The guide's principle of "when in doubt, treat it as a red line" is the fallback strategy, but it ultimately depends on the model's judgment.

### Q6: Does Skill inspection only need to be done once?

No. Re-inspection is needed when: a Skill is updated, the OpenClaw engine is updated, a Skill exhibits abnormal behavior, or the audit report shows a Skill fingerprint mismatch.

### Q7: What if the OpenClaw engine itself has a security vulnerability?

This guide's protective measures are all built on the assumption that "the engine itself is trustworthy" and cannot defend against engine-level vulnerabilities. Stay informed through OpenClaw's official security advisories and update the engine promptly.

## 📝 License
This project is [MIT](LICENSE) licensed.
