# harmonyos-dev

> An AI-agent skill for **HarmonyOS project scaffolding** and **environment verification** — quickly initialize a Standard ArkTS or Native C++ project with all toolchain checks, dependency install, and git init handled automatically.

[![HarmonyOS 6.0](https://img.shields.io/badge/HarmonyOS-6.0%20(API%2020)-blue)](#api-level-reference)
[![AI Agent Skill](https://img.shields.io/badge/AI%20Agent-Skill-orange)](#what-is-this-skill)

---

## Table of Contents

- [What is this Skill?](#what-is-this-skill)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Templates](#templates)
  - [Standard ArkTS Template](#standard-arkts-template)
  - [Native C++ Template](#native-c-template)
- [Demo](#demo)
- [Project Structure](#project-structure)
- [API Level Reference](#api-level-reference)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

---

## What is this Skill?

This skill has **two core goals**:

1. **Project Scaffolding** — Copy a ready-to-build HarmonyOS template (Standard ArkTS or Native C++) into the current directory, install dependencies, and initialize git.
2. **Environment Verification** — Check that the required toolchain (`node`, `ohpm`, `hvigorw`, `codelinter`) is installed and configured correctly before you waste time on a broken build.

Instead of manually juggling `build-profile.json5`, `oh-package.json5`, SDK versions, and signing configs, you just ask the agent to set it up.

It works with any AI coding agent that supports the skill/instruction file convention (e.g., Gemini CLI, GitHub Copilot, Cursor, Claude Code, etc.).

---

## Features

### Core (Scaffolding & Verification)

| Feature | Description |
| :--- | :--- |
| **Standard ArkTS Template** | Pure ArkTS project, ready to build, targeting HarmonyOS 6.0 (API 20) |
| **Native C++ Template** | ArkTS + NAPI (C/C++) project with CMake integration |
| **Environment Check** | Validates `node`, `ohpm`, `hvigorw`, `codelinter` are on PATH and working |
| **Dependency Install** | Runs `ohpm install` and validates `oh-package.json5` automatically |
| **Build Verification** | Initial build attempt to catch configuration issues immediately |
| **Git Init** | Auto `git init` + initial commit on successful scaffold |
| **SDK Targeting** | Templates pre-configured for **HarmonyOS 6.0 / API Level 20** |

### Supplementary (Bundled References)

The skill also ships `references/` docs that the AI agent can use as context during development:

| Reference | What it covers |
| :--- | :--- |
| `architecture.md` | Clean Architecture guidance (Domain / Data / Presentation layers) |
| `arkts-strict.md` | ArkTS strict-mode constraints and best practices |
| `troubleshooting.md` | Common build & environment issues and fixes |
| `agency-workflow.md` | Agentic lifecycle patterns (`TASKS.md`, `/cycle` workflow) |

---

## Prerequisites

Before installing, make sure these tools are available on your `PATH`:

| Tool | Purpose | Verify |
| :--- | :--- | :--- |
| **Node.js** (≥ 16) | Runtime for build tooling | `node -v` |
| **ohpm** | OpenHarmony Package Manager | `ohpm -v` |
| **hvigorw** | HarmonyOS Build Wrapper | `hvigorw -v` |
| **codelinter** | Code quality checker | `codelinter -h` |
| **AI Agent** | Any agent supporting skill files (Gemini CLI, Copilot, Cursor, etc.) | — |

> **Tip:** All tools above ship as part of the [HarmonyOS SDK / DevEco Studio](https://developer.huawei.com/consumer/en/deveco-studio/) command-line tools package. Add `<SDK>/command-line-tools/bin` to your `PATH`.

---

## Installation

### Option A — Use the Skills CLI (Recommended & Easiest)

The fastest way to install this skill into your agent (Gemini CLI, etc.) is via the `skills` CLI:

```bash
npx skills add imansmallapple/Harmonyos-dev-skill
```

To see what's available in the repository before installing:

```bash
npx skills add imansmallapple/Harmonyos-dev-skill --list
```

### Option B — Clone into your agent's skills directory

Clone into the skills/instructions directory used by your AI agent:

```bash
# Example: Gemini CLI
cd ~/.gemini/skills && git clone https://github.com/<your-username>/harmonyos-dev.git

# Example: GitHub Copilot (VS Code) — use as a workspace instruction
cd .github && git clone https://github.com/<your-username>/harmonyos-dev.git

# Example: Cursor
cd .cursor/skills && git clone https://github.com/<your-username>/harmonyos-dev.git
```

The skill is automatically picked up on the next agent session.

### Option B — Manual copy

1. Download or clone the repository anywhere on your machine.
2. Copy the `harmonyos-dev/` folder into your agent's skill directory.

```powershell
# Windows (PowerShell) — Gemini CLI example
Copy-Item -Recurse .\harmonyos-dev "$env:USERPROFILE\.gemini\skills\harmonyos-dev"
```

```bash
# macOS / Linux — Gemini CLI example
cp -r ./harmonyos-dev ~/.gemini/skills/harmonyos-dev
```

> Adjust the destination path based on your specific AI agent.

---

## Quick Start

### 1. Scaffold a Standard ArkTS Project

```
You:  "Initialize a new HarmonyOS project"
```

The skill will:
1. Run pre-flight environment checks (Node, ohpm, hvigorw, codelinter)
2. Copy the **Standard ArkTS** template into the current directory
3. Run `ohpm install` to resolve dependencies
4. Execute `node scripts/check_env.cjs` for verification
5. Initialize a git repository with an initial commit

### 2. Scaffold a Native C++ Project

```
You:  "Create a new Native C++ HarmonyOS app"
```

Same workflow, but uses the Native C++ template with NAPI bindings and CMake configuration.

### 3. Build & Verify an Existing Project

```
You:  "Build and verify the project"
```

Runs environment checks, installs dependencies, and validates the build configuration.

---

## Templates

### Standard ArkTS Template

A pure **ArkTS** application targeting HarmonyOS 6.0 (API 20).

```
project-root/
├── AppScope/
│   ├── app.json5                  # Bundle name, version, icons
│   └── resources/                 # App-level resources
├── entry/
│   └── src/main/
│       ├── ets/
│       │   ├── entryability/      # UIAbility entry point
│       │   └── pages/
│       │       └── Index.ets      # Default page
│       ├── resources/             # Module resources
│       └── module.json5           # Module manifest
├── build-profile.json5            # SDK version & build config
├── oh-package.json5               # Dependency manifest
└── hvigorfile.ts                  # Build script
```

**Default `Index.ets`:**

```typescript
@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    RelativeContainer() {
      Text(this.message)
        .fontSize($r('app.float.page_text_font_size'))
        .fontWeight(FontWeight.Bold)
        .alignRules({
          center: { anchor: '__container__', align: VerticalAlign.Center },
          middle: { anchor: '__container__', align: HorizontalAlign.Center }
        })
        .onClick(() => {
          this.message = 'Welcome';
        })
    }
    .height('100%')
    .width('100%')
  }
}
```

### Native C++ Template

An **ArkTS + Native API (C/C++)** application with NAPI bindings and CMake build integration.

Everything in the Standard template, plus:

```
entry/
├── libs/arm64-v8a/                # Pre-built native libraries
└── src/main/cpp/
    ├── CMakeLists.txt             # CMake build config
    ├── napi_init.cpp              # NAPI module registration
    └── types/                     # Type declarations for JS↔C++ bridge
```

**Example — calling native code from ArkTS:**

```typescript
import testNapi from 'libentry.so';

// In your @Component:
hilog.info(0x0000, 'testTag', 'NAPI 2 + 3 = %{public}d', testNapi.add(2, 3));
```

**The corresponding C++ side (`napi_init.cpp`):**

```cpp
#include "napi/native_api.h"

static napi_value Add(napi_env env, napi_callback_info info) {
    size_t argc = 2;
    napi_value args[2] = {nullptr};
    napi_get_cb_info(env, info, &argc, args, nullptr, nullptr);

    double value0, value1;
    napi_get_value_double(env, args[0], &value0);
    napi_get_value_double(env, args[1], &value1);

    napi_value sum;
    napi_create_double(env, value0 + value1, &sum);
    return sum;
}
```

---

## Demo

### Full Workflow Example

```
┌─────────────────────────────────────────────────────────────────┐
│  You:  Create a new HarmonyOS app in ./my-app                  │
│                                                                 │
│  Agent:                                                         │
│  ✅ Node.js: Found (v18.20.2)                                   │
│  ✅ ohpm: Found (5.0.11)                                        │
│  ✅ Hvigor Wrapper: Found (5.18.1)                              │
│  ✅ CodeLinter: Found                                            │
│                                                                 │
│  📁 Copying Standard ArkTS template to ./my-app ...             │
│  📦 Running ohpm install ...                                    │
│  🔨 Running environment check ...                               │
│  ✅ Build verification passed!                                   │
│  🎉 Git initialized with initial commit.                         │
│                                                                 │
│  Your HarmonyOS project is ready at ./my-app                    │
│  Open it in DevEco Studio or continue building.                 │
└─────────────────────────────────────────────────────────────────┘
```

### Natural Language Commands

| What you say | What happens |
| :--- | :--- |
| *"Initialize a new HarmonyOS project"* | Scaffolds Standard ArkTS template |
| *"Create a new Native C++ HarmonyOS app"* | Scaffolds Native C++ template |
| *"Create a HarmonyOS app called my-app"* | Scaffolds into `./my-app` directory |
| *"Build and verify the project"* | Runs env checks + dependency install |
| *"Check the build environment"* | Runs `check_env.cjs` diagnostics |

---

## Project Structure

```
harmonyos-dev/
├── SKILL.md                        # Skill definition (read by AI agents)
├── README.md                       # This file
├── assets/
│   ├── harmonyos-project-template/ # Standard ArkTS template
│   │   ├── AppScope/               # App-level config & resources
│   │   ├── entry/                  # Main module (pages, abilities)
│   │   ├── hvigor/                 # Build tool config
│   │   ├── build-profile.json5     # SDK version targeting (API 20)
│   │   ├── oh-package.json5        # Dependencies (hypium, hamock)
│   │   └── ...
│   └── nativec-template/           # Native C++ template
│       ├── entry/src/main/cpp/     # C++ source & CMakeLists.txt
│       ├── entry/libs/arm64-v8a/   # Native library output
│       └── ...                     # (same structure as standard +)
├── references/
│   ├── architecture.md             # Clean Architecture guide
│   ├── arkts-strict.md             # ArkTS strict-mode constraints
│   ├── troubleshooting.md          # Common issues & fixes
│   └── agency-workflow.md          # Agentic lifecycle patterns
└── scripts/
    └── check_env.cjs               # Environment verification script
```

---

## API Level Reference

| HarmonyOS Version | API Level | Status |
| :--- | :--- | :--- |
| 4.0 | 10 | Legacy |
| 4.1 | 11 | Legacy |
| 5.0.0 | 12 | Stable |
| 5.0.1 | 13 | Stable |
| 5.0.2 | 14 | Stable |
| 5.0.3 | 15 | Stable |
| 5.1.0 | 18 | Stable |
| 5.1.1 | 19 | Stable |
| **6.0** | **20** | **Current (default)** |

> **Note:** If a version string includes a number in parentheses such as `6.0.0(20)`, the parenthesized number is the API Level.

---

## Troubleshooting

<details>
<summary><strong>"ohpm not found"</strong></summary>

The OpenHarmony Package Manager is not on your PATH.

**Fix:** Add `<HarmonyOS-SDK>/command-line-tools/bin` to your system PATH.
</details>

<details>
<summary><strong>"hvigorw is not recognized"</strong></summary>

The build wrapper script is missing or not executable.

**Fix:**
1. Check if `node_modules/.bin/hvigor` works as a fallback.
2. Install globally: `npm install -g @ohos/hvigor`.
3. Re-open the project in DevEco Studio to regenerate wrappers.
</details>

<details>
<summary><strong>Build fails with "Command failed with exit code 1"</strong></summary>

Usually a compilation error in ArkTS files.

**Fix:**
1. Run `hvigorw assembleHap --info` for detailed logs.
2. Check `oh-package.json5` for missing dependencies.
3. Run `ohpm install` to re-link packages.
</details>

<details>
<summary><strong>"SigningConfigs profile" warning</strong></summary>

No signing certificate is configured.

**Fix:** Safe to ignore for emulators. For physical devices, configure signing in `build-profile.json5`.
</details>

See [references/troubleshooting.md](references/troubleshooting.md) for the full troubleshooting guide.

---

## Contributing

Contributions are welcome! Here are some ways to help:

- **Add new templates** — e.g., a Service extension template, a Widget template
- **Improve references** — expand the architecture, ArkTS, or troubleshooting docs
- **Enhance scripts** — add more diagnostic checks to `check_env.cjs`
- **Report issues** — file bugs or feature requests via GitHub Issues

### Development

```bash
# Clone the repo
git clone https://github.com/<your-username>/harmonyos-dev.git

# Place it into your agent's skills directory (example: Gemini CLI)
cp -r harmonyos-dev ~/.gemini/skills/

# Start your AI agent and test
> "Initialize a new HarmonyOS project in ./test-app"
```

---

<p align="center">
  <strong>Built for the HarmonyOS community</strong><br/>
  <em>Works with any AI coding agent</em>
</p>
