# Trae IDE Setup

This guide explains how to set up Planning with Files in Trae IDE.

## What is Trae IDE?

Trae IDE is a modern AI-powered development environment that supports Skills for customizing AI behavior.

## Installation

### Method 1: Clone to Project (Recommended)

Clone the repository into your project's `.trae/skills/` directory:

```bash
# Create skills directory if it doesn't exist
mkdir -p .trae/skills

# Clone or copy the planning-with-files skill
cp -r /path/to/planning-with-files/.trae/skills/planning-with-files .trae/skills/
```

### Method 2: Global Installation

Install globally to use across all projects:

**macOS/Linux:**
```bash
mkdir -p ~/.trae/skills
cp -r /path/to/planning-with-files/.trae/skills/planning-with-files ~/.trae/skills/
```

**Windows (PowerShell):**
```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.trae\skills"
Copy-Item -Recurse -Path "path\to\planning-with-files\.trae\skills\planning-with-files" -Destination "$env:USERPROFILE\.trae\skills\"
```

## Directory Structure

After installation, your project should have:

```
your-project/
├── .trae/
│   └── skills/
│       └── planning-with-files/
│           ├── SKILL.md          # Main skill definition
│           ├── templates/        # Planning file templates
│           │   ├── task_plan.md
│           │   ├── findings.md
│           │   └── progress.md
│           ├── scripts/          # Helper scripts
│           │   ├── init-session.sh
│           │   ├── init-session.ps1
│           │   ├── check-complete.sh
│           │   └── check-complete.ps1
│           ├── examples.md       # Usage examples
│           └── reference.md      # Manus principles
├── task_plan.md                  # Created when you start planning
├── findings.md                   # Created when you start planning
└── progress.md                   # Created when you start planning
```

## Usage

### Starting a Planning Session

Tell the AI to use the planning-with-files skill:

```
使用 planning-with-files 技能开始规划任务
```

Or describe a complex task and the AI will automatically use the skill:

```
帮我创建一个用户认证系统，包含登录、注册和密码重置功能
```

### Initializing Planning Files

Use the provided script to create planning files:

**macOS/Linux:**
```bash
./.trae/skills/planning-with-files/scripts/init-session.sh my-project
```

**Windows (PowerShell):**
```powershell
.\.trae\skills\planning-with-files\scripts\init-session.ps1 my-project
```

### Checking Completion Status

Verify all phases are complete:

**macOS/Linux:**
```bash
./.trae/skills/planning-with-files/scripts/check-complete.sh
```

**Windows (PowerShell):**
```powershell
.\.trae\skills\planning-with-files\scripts\check-complete.ps1
```

## The 3-File Pattern

When using this skill, three files are created in your project root:

| File | Purpose |
|------|---------|
| `task_plan.md` | Track phases, progress, and decisions |
| `findings.md` | Store research, discoveries, and knowledge |
| `progress.md` | Log session activities and test results |

## Core Rules

1. **Create Plan First** — Never start a complex task without `task_plan.md`
2. **2-Action Rule** — Save findings after every 2 view/browser/search operations
3. **Read Before Decide** — Re-read plan before major decisions
4. **Update After Act** — Mark phases complete and log errors
5. **Log ALL Errors** — Track attempts and resolutions

## When to Use

**Use for:**
- Multi-step tasks (3+ steps)
- Research tasks
- Building/creating projects
- Tasks spanning many tool calls

**Skip for:**
- Simple questions
- Single-file edits
- Quick lookups

## Example Workflow

1. **Start**: Tell AI to use the skill
2. **Plan**: AI creates `task_plan.md` with phases
3. **Execute**: AI works through phases, updating files
4. **Track**: Findings go to `findings.md`, progress to `progress.md`
5. **Complete**: All phases marked complete

## Troubleshooting

### Skill Not Loading

Ensure the SKILL.md file exists in the correct location:
```
.trae/skills/planning-with-files/SKILL.md
```

### Files Not Created

Run the init script manually:
```bash
./.trae/skills/planning-with-files/scripts/init-session.sh
```

### Phase Status Not Updating

Manually edit `task_plan.md` and change:
- `**Status:** pending` → `**Status:** in_progress`
- `**Status:** in_progress` → `**Status:** complete`

## Additional Resources

- [Quick Start Guide](quickstart.md)
- [Workflow Diagram](workflow.md)
- [Examples](../.trae/skills/planning-with-files/examples.md)
- [Manus Principles Reference](../.trae/skills/planning-with-files/reference.md)

## Version

- **Skill Version**: 2.15.0
- **Trae IDE Support**: Added in v2.16.0
