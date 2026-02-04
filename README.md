# GROUP 107 Claude Code Onboarding Guide

## Introduction

### What is Claude Code?
Claude Code is an AI-powered coding assistant that runs in your terminal. It understands your codebase, writes code, runs tests, handles Git workflows, and assists with development tasks through natural language commands.

### Why are we adopting it?
- Accelerate development without sacrificing code quality
- Maintain consistent coding standards across projects
- Reduce time on repetitive tasks (boilerplate, tests, documentation)
- Provide 24/7 coding assistance for all skill levels

### What this guide covers
1. Getting access and installation
2. Project setup
3. Daily usage
4. Security rules (mandatory)
5. Available sub-agents
6. Best practices
7. Troubleshooting

---

## 1. Getting Access

### Step 1: Create Anthropic Account
1. Go to [claude.ai](https://claude.ai)
2. Sign up with your **company email**
3. Verify your email address

### Step 2: Subscribe to Pro Plan
1. Log into claude.ai
2. Go to Settings → Subscription
3. Subscribe to **Pro** plan
4. Save your receipt for expense reimbursement

---

## 2. Installation

### Prerequisites
- Node.js 18+ installed
- Git installed and configured
- Terminal access (bash, zsh, PowerShell)

### Install Claude Code

**macOS / Linux:**
```bash
npm install -g @anthropic-ai/claude-code
```

**Windows:**
```powershell
npm install -g @anthropic-ai/claude-code
```

### Authenticate
```bash
claude auth login
```
Follow the browser prompt to log in with your Anthropic account.

### Verify Installation
```bash
claude --version
```

### Update Claude Code
```bash
npm update -g @anthropic-ai/claude-code
```
Keep Claude Code updated for latest features and security patches.

### Install Context7 MCP (MANDATORY)

Context7 provides real-time library documentation to prevent outdated code suggestions.

**Step 1: Add Context7 MCP server**
```bash
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

**Step 2: Verify installation**
```bash
claude mcp list
```
You should see `context7` in the list.

**Why it's mandatory:**
- Prevents AI from suggesting deprecated APIs
- Provides version-specific documentation
- Eliminates hallucinated methods that don't exist
- Ensures code works with current library versions

---

## 3. Project Setup

Every project must be configured before using Claude Code. This ensures consistent behavior and security.

### Step 1: Get Project Templates

Clone or download the company templates:
```bash
# From company template repository
git clone https://github.com/Israel-IT/claude-templates ~/claude-templates
```



### Step 2: Add Configuration Files

The template package contains:
```
claude-templates/
├── base/
│   └── CLAUDE.base.md      → Copy to project root as CLAUDE.md
├── commands/ (or commands_agents/)
│   ├── frontend.md         → Sub-agent: /frontend
│   ├── backend.md          → Sub-agent: /backend
│   ├── qa.md               → Sub-agent: /qa
│   ├── review.md           → Sub-agent: /review
│   ├── architect.md        → Sub-agent: /architect
│   ├── devops.md           → Sub-agent: /devops
│   ├── manager.md          → Sub-agent: /manager
│   └── security.md         → Sub-agent: /security
├── settings/
│   └── settings.json       → Security permissions (deny rules)
└── docs/
    └── ONBOARDING.md       → This guide
```

**macOS / Linux:**
```bash
# Create directories
mkdir -p .claude/commands

# Copy base configuration to project root
cp ~/claude-templates/base/CLAUDE.base.md ./CLAUDE.md

# Copy security settings
cp ~/claude-templates/settings/settings.json .claude/

# Copy sub-agent definitions (folder may be named "commands" or "commands_agents")
cp ~/claude-templates/commands/*.md .claude/commands/
```

**Windows PowerShell:**
```powershell
# Create directories
New-Item -ItemType Directory -Force -Path .claude\commands

# Copy base configuration to project root
Copy-Item C:\path\to\claude-templates\base\CLAUDE.base.md -Destination .\CLAUDE.md

# Copy security settings
Copy-Item C:\path\to\claude-templates\settings\settings.json -Destination .claude\

# Copy sub-agent definitions (folder may be named "commands" or "commands_agents")
Copy-Item C:\path\to\claude-templates\commands\*.md -Destination .claude\commands\
# OR if folder is named "commands_agents":
Copy-Item C:\path\to\claude-templates\commands_agents\*.md -Destination .claude\commands\
```

**Note:** Replace `C:\path\to\` with your actual download location (e.g., `C:\Users\YourName\Downloads\`).

**After copying, your project should have:**
```
your-project/
├── CLAUDE.md                    ← Base rules (in project root)
└── .claude/
    ├── settings.json            ← Security permissions
    └── commands/
        ├── frontend.md          ← /frontend sub-agent
        ├── backend.md           ← /backend sub-agent
        ├── qa.md                ← /qa sub-agent
        ├── review.md            ← /review sub-agent
        ├── architect.md         ← /architect sub-agent
        ├── devops.md            ← /devops sub-agent
        ├── manager.md           ← /manager sub-agent
        └── security.md          ← /security sub-agent
```

### Step 3: Customize CLAUDE.md for Your Project

The `CLAUDE.md` file is a template. **You must customize it** for each project's specific needs.

**Open CLAUDE.md** and look for sections marked with `<!-- CUSTOMIZE -->` comments. These indicate areas that typically need project-specific adjustments.

**Common customizations:**

| Area | Example Customization |
|------|----------------------|
| **Jira Key Format** | Change `[PROJECT-123]` to your actual project key: `[FINTRACK-123]`, `[NOVA-456]` |
| **Jira Strategy** | Some projects use subtasks, epics, or specific workflows |
| **Branch Naming** | `feature/PROJECT-123-description` or `feat/ticket-description` |
| **PR Size Limit** | Adjust 400 lines if project needs larger PRs |
| **Test Coverage** | Set specific threshold: 80%, 90%, or per-module requirements |
| **Tech Stack Commands** | Add actual build/test/run commands for your project |
| **Client Standards** | Add any client-imposed coding rules or restrictions |
| **Code Review Rules** | Require specific reviewers for certain file types |
| **Documentation Language** | If client requires non-English documentation |
| **Deployment Restrictions** | Specific branches that trigger deployments |

**Example customizations:**

**Jira Configuration (if using different workflow):**
```markdown
## Commit Rules
- Commits must reference Jira: `[FINTRACK-123] Add login endpoint`
- Bug fixes use: `[FINTRACK-123] fix: Resolve null pointer in UserService`
- Features use: `[FINTRACK-123] feat: Add password reset flow`
```

**Branch Strategy (if using GitFlow):**
```markdown
## Branch Naming
- Features: `feature/FINTRACK-123-short-description`
- Bugfixes: `bugfix/FINTRACK-123-short-description`
- Hotfixes: `hotfix/FINTRACK-123-short-description`
- Release: `release/v1.2.0`
```

**Client-Specific Rules:**
```markdown
## Client-Specific Rules
- All API responses must include `X-Request-Id` header
- No third-party analytics libraries allowed
- All dates must be UTC in ISO 8601 format
- Maximum response time: 200ms for GET endpoints
```

**Testing Requirements (stricter):**
```markdown
## Testing Requirements
- Minimum 85% code coverage
- All public methods must have unit tests
- Integration tests required for all API endpoints
- Performance tests required for endpoints handling >100 requests/sec
```

### Step 4: Initialize Project Context

Run in your project root:
```bash
claude /init
```

Claude will analyze your project and suggest additions to CLAUDE.md. Review and approve relevant suggestions.

### Step 5: Commit Configuration

```bash
git add CLAUDE.md .claude/
git commit -m "[PROJECT-XXX] Add Claude Code configuration"
```

**Note:** `.claude/settings.json` contains security rules and should be committed.

---

## 4. Daily Usage

### Starting Claude Code

Navigate to your project and start Claude:
```bash
cd /path/to/project
claude
```

### Basic Commands

| Command | Description |
|---------|-------------|
| `/help` | Show all available commands |
| `/init` | Initialize or update CLAUDE.md |
| `/compact` | Compress conversation context |
| `/clear` | Clear conversation history |
| `/status` | Show current session status |
| `/quit` or `Ctrl+C` | Exit Claude Code |

### Asking Claude to Code

**Be specific about what you want:**
```
# Good
Create a UserService class with methods for GetById, Create, Update, Delete. 
Use repository pattern. Include input validation.

# Bad
Make a user service
```

**Reference files directly:**
```
Look at src/Services/OrderService.cs and create a similar service for Products
```

**Request tests:**
```
Create unit tests for the ProductService class covering all public methods
```

### Using Context7 (MANDATORY)

When working with any library, **always** add `use context7` to your prompt:

```
Create a REST API endpoint using Express.js with validation. use context7

Implement JWT authentication in .NET 8. use context7

Add React Query for data fetching with caching. use context7
```

**Why:** Without Context7, Claude may suggest:
- Deprecated methods
- APIs that don't exist
- Syntax from older versions

Context7 fetches current, version-specific documentation automatically.

### Using Extended Thinking Mode

For complex tasks, use thinking keywords to allocate more reasoning:

| Keyword | When to Use |
|---------|-------------|
| `think` | Moderate complexity, routine refactoring |
| `think hard` | New features, debugging, multiple considerations |
| `ultrathink` | Architecture decisions, critical code, when stuck |

**Examples:**

```
think. Review this service class and suggest improvements.

think hard. Implement a caching layer for our API with Redis.

ultrathink. Analyze the entire authentication flow and propose a security-focused refactoring plan. Don't code yet.
```

**Best practice workflow for complex tasks:**
```
1. You: ultrathink. Analyze this codebase and create a plan for [feature]. Don't code yet.
2. Claude: [Provides detailed plan]
3. You: Approved, implement step 1
4. Claude: [Implements]
5. You: Run tests
```

**When NOT to use thinking keywords:**
- Simple fixes (typos, formatting)
- Quick lookups
- Clear step-by-step instructions already provided
- Slows down simple tasks unnecessarily

### Using Sub-Agents

Invoke specialized behavior with commands:

| Command | Use For |
|---------|---------|
| `/frontend` | React/Angular/Vue component work |
| `/backend` | API, services, data access |
| `/qa` | Writing tests, test review |
| `/review` | Code review feedback |
| `/architect` | Design decisions, architecture |
| `/devops` | CI/CD, Docker, infrastructure |
| `/manager` | Task breakdown, estimates, tickets |
| `/security` | Security audit, vulnerability check |

**Example:**
```
/security Review the authentication flow in AuthController.cs
```

```
/qa Write comprehensive tests for OrderService including edge cases
```

```
/manager Break down this feature into Jira tickets: [feature description]
```

### The # Key Shortcut

Press `#` to add instructions that Claude will remember for the session:
```
# Always use async/await in this project
# Our API returns camelCase JSON
# Use xUnit for tests, not NUnit
```

These can be added to CLAUDE.md for permanent memory.

---

## 5. Security Rules (MANDATORY)

### ❌ NEVER Do This

| Action | Risk |
|--------|------|
| Share credentials with Claude | Credentials may be logged or exposed |
| Ask Claude to read .env files | Contains secrets |
| Paste API keys in prompts | Keys become part of conversation |
| Ask Claude to commit secrets | Secrets end up in Git history |
| Bypass security settings | Exposes project to risk |
| Run Claude as admin/root | Excessive permissions |

### ✅ ALWAYS Do This

| Action | Reason |
|--------|--------|
| Use environment variables for secrets | Keeps secrets out of code |
| Verify security settings exist | `.claude/settings.json` must be present |
| Review generated code for hardcoded values | AI may accidentally include placeholders |
| Run security review before auth changes | `/security` command catches issues |
| Report security concerns to Tech Lead | Escalate immediately |

### If Claude Asks for Credentials

**Do not provide them.** Instead:
```
Credentials are managed through environment variables. 
Use placeholder like "YOUR_API_KEY_HERE" in the code.
```

### If You Accidentally Expose a Secret

1. **Immediately** rotate the compromised credential
2. Notify your Tech Lead
3. Check Git history and remove if committed
4. Document the incident

---

## 6. Required Workflow

Claude is configured to follow this workflow. You should too.

### Before Coding
1. **Orient** — Understand the task and affected files
2. **Plan** — Ask Claude for a plan first: "Plan how to implement [feature], don't code yet"
3. **Approve** — Review the plan before proceeding

### During Coding
4. **Implement** — Let Claude write code in small chunks
5. **Review** — Check each change before accepting
6. **Test** — Run tests after each significant change

### After Coding
7. **Verify** — Full test suite passes
8. **Review** — Use `/review` for self-review before PR
9. **Commit** — Include Jira ticket in message

### Example Session

```
You: Plan how to add pagination to the GetProducts endpoint, don't code yet

Claude: [Provides 5-bullet plan]

You: Approved, implement step 1

Claude: [Implements first step]

You: Run the tests

Claude: [Runs tests, shows results]

You: Implement step 2
...
```

---

## 7. Best Practices

### Do

- **Start sessions with context**: "I'm working on [ticket], which involves [brief description]"
- **Request small changes**: Easier to review and less likely to have errors
- **Verify generated code**: AI makes mistakes—always review
- **Use sub-agents**: They have specialized knowledge for their domain
- **Commit frequently**: Don't let AI changes pile up
- **Run tests often**: Catch issues early
- **Ask for explanations**: "Explain why you chose this approach"

### Don't

- **Trust blindly**: AI can hallucinate APIs, methods, or patterns that don't exist
- **Skip reviews**: AI-generated code needs the same review as human code
- **Overwhelm with requests**: One task at a time works better
- **Ignore warnings**: If Claude says something is risky, listen
- **Fight the workflow**: Orient → Plan → Implement → Prove → Finish

### Context Management

Claude has limited memory within a session. For long sessions:

```bash
/compact   # Summarize and compress context
```

For complex features spanning multiple sessions, document progress in:
- Jira ticket comments
- A `docs/progress.md` file Claude can read

---

## 8. Sub-Agent Reference

### /frontend
**Use for:** Component creation, styling, state management, accessibility
**Auto-triggers:** Working in `/components/`, `.tsx`, `.vue`, `.scss` files
**Key standards:**
- One component per file
- Props interface at top
- Mobile-first responsive design
- Accessibility (ARIA, keyboard nav)

### /backend
**Use for:** APIs, services, data access, business logic
**Auto-triggers:** Working in `/controllers/`, `/services/`, `.cs`, `.java` files
**Key standards:**
- Layered architecture (Controller → Service → Repository)
- Parameterized queries only
- Async/await for I/O
- Proper error handling

### /qa
**Use for:** Writing tests, reviewing test coverage, verifying bug fixes
**Auto-triggers:** Working in `/tests/`, `.test.*`, `.spec.*` files
**Key standards:**
- AAA pattern (Arrange, Act, Assert)
- One assertion concept per test
- No sleeps or real network calls
- Deterministic tests only

### /review
**Use for:** Pre-PR self-review, code quality feedback
**Invoke explicitly:** `/review [file or description]`
**Output:** Structured review with BLOCKER/MAJOR/MINOR/NIT categories

### /architect
**Use for:** Design decisions, new service planning, technology selection
**Invoke explicitly:** `/architect [design question]`
**Output:** Trade-off analysis, decision documentation

### /devops
**Use for:** CI/CD pipelines, Docker, infrastructure
**Auto-triggers:** Working in `.github/workflows/`, `Dockerfile`, `.tf` files
**Key standards:**
- Multi-stage Docker builds
- Secrets via GitHub Secrets only
- Health check endpoints required

### /manager
**Use for:** Task breakdown, estimation, Jira ticket writing, status reports
**Invoke explicitly:** `/manager [request]`
**Output:** Structured tickets, estimates with reasoning, risk identification

### /security
**Use for:** Security review, vulnerability detection
**Invoke explicitly:** `/security [code or component to review]`
**Output:** Vulnerability report with severity, remediation steps
**When to use:**
- Before PR for auth/authz changes
- When handling user input
- When adding new dependencies
- Periodic codebase audit

---

## 9. Troubleshooting

### "Claude doesn't know about my project structure"

**Solution:** Run `/init` or add details to CLAUDE.md manually.

### "Claude is ignoring my coding standards"

**Solution:** 
1. Check CLAUDE.md exists in project root
2. Check standards are clearly written
3. Remind Claude: "Follow the project standards in CLAUDE.md"

### "Claude wants to read .env file"

**Solution:** This should be blocked by settings.json. Verify `.claude/settings.json` exists and contains deny rules. Never approve .env access.

### "Claude made up an API that doesn't exist"

**Solution:** AI hallucination. Always verify:
```
Verify this API exists before implementing. Check the actual package documentation.
```

### "Session is getting slow"

**Solution:** Context window filling up. Run `/compact` or start new session.

### "Claude keeps asking for permission"

**Solution:** Expected for operations in the "ask" list (git push, npm install). Review and approve or deny each time.

### "Generated code doesn't compile"

**Solution:** 
1. Share the exact error with Claude
2. Ask Claude to fix based on error message
3. If persistent, check if Claude is using wrong framework version

### "I hit usage limits"

**Solution:** Pro plan has generous limits that reset every 5 hours. If consistently hitting limits:
- Use `/compact` more often
- Break work into smaller sessions
- Check if prompts can be more concise
- Reduce use of `ultrathink` for simple tasks

### "Context7 is not working"

**Solution:**
1. Verify installation: `claude mcp list`
2. If not listed, reinstall: `claude mcp add context7 -- npx -y @upstash/context7-mcp`
3. Ensure Node.js 18+ is installed
4. Check you included `use context7` in your prompt

### "Thinking mode seems slow"

**Solution:** Extended thinking takes more time by design. Use appropriately:
- Simple tasks → no keyword
- Moderate tasks → `think`
- Complex tasks → `think hard` or `ultrathink`

Don't use `ultrathink` for everything—it adds latency and cost.

---

## 10. Getting Help

### Self-Service
- This guide (you're here)
- Claude Code docs: [code.claude.ai/docs](https://code.claude.ai/docs)


### Team Support
- **Tech questions:** Ask in #dev-claude-code Slack channel
- **Setup issues:** Contact Myroslav Pap
- **Security concerns:** Notify Tech Lead or Project Manager immediately

### Escalation
| Issue | Contact |
|-------|---------|
| Security incident | Tech Lead → Management |
| Billing/subscription | HR |
| Standards questions | project team lead/ Myroslav Pap|

---

## 11. Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                 CLAUDE CODE QUICK REFERENCE                  │
├─────────────────────────────────────────────────────────────┤
│ START SESSION        │ cd /project && claude                │
│ GET HELP             │ /help                                │
│ INITIALIZE PROJECT   │ /init                                │
│ COMPRESS CONTEXT     │ /compact                             │
│ CHECK COST           │ /cost                                │
│ EXIT                 │ /quit or Ctrl+C                      │
├─────────────────────────────────────────────────────────────┤
│ MANDATORY TOOLS                                             │
│ Context7:  Add "use context7" when using any library        │
├─────────────────────────────────────────────────────────────┤
│ THINKING MODES                                              │
│ think       │ Moderate tasks (~4K tokens)                   │
│ think hard  │ Complex tasks (~10K tokens)                   │
│ ultrathink  │ Architecture/critical (~32K tokens)           │
├─────────────────────────────────────────────────────────────┤
│ SUB-AGENTS                                                  │
│ /frontend  │ /backend   │ /qa       │ /review              │
│ /architect │ /devops    │ /manager  │ /security            │
├─────────────────────────────────────────────────────────────┤
│ WORKFLOW: Orient → Plan → Implement → Prove → Finish        │
├─────────────────────────────────────────────────────────────┤
│ ❌ NEVER: Share credentials, read .env, bypass security     │
│ ✅ ALWAYS: Review code, run tests, use Jira key in commits  │
│ ✅ ALWAYS: Use "use context7" for library work              │
└─────────────────────────────────────────────────────────────┘
```

---

## Changelog

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-02-02 | Myroslav Pap | Initial version |

---

*Questions or feedback about this guide? Contact Myroslav Pap.*

