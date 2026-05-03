# ⚡ Imperat AI Skill

> **Generate correct, version-aware [Imperat](https://github.com/MeveraStudios/imperat) commands directly from Claude — no more guessing at the API.**

A [Claude Code skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that bundles the full **v3** and **v4** Imperat documentation and teaches Claude to:

1. **Auto-detect** which Imperat version your project uses (by reading your `pom.xml` or `build.gradle`).
2. **Read only the docs that matter** for your task (annotations, platforms, argument types, etc.).
3. **Emit a complete command class** in Java or Kotlin with the right annotations, registration call, and platform-specific glue.

<p align="center">
  <img alt="Java" src="https://img.shields.io/badge/Java-17%2B-orange?logo=openjdk&logoColor=white">
  <img alt="Kotlin" src="https://img.shields.io/badge/Kotlin-supported-7F52FF?logo=kotlin&logoColor=white">
  <img alt="Imperat" src="https://img.shields.io/badge/Imperat-v3%20%26%20v4-e11d48">
  <img alt="License" src="https://img.shields.io/badge/license-MIT-blue">
</p>

---

## 🚀 Quick install

Pick the path that matches your setup. Most people want **Option A**.

### Option A — Drop it in your global skills folder (recommended)

<details open>
<summary><strong>macOS / Linux</strong></summary>

```bash
git clone https://github.com/MeveraStudios/Imperat-AI-Skill.git \
  ~/.claude/skills/imperat-commands
```

</details>

<details>
<summary><strong>Windows (PowerShell)</strong></summary>

```powershell
git clone https://github.com/MeveraStudios/Imperat-AI-Skill.git `
  "$env:USERPROFILE\.claude\skills\imperat-commands"
```

</details>

That's it. Restart Claude Code (or just open a new session) and the `imperat-commands` skill shows up in `/skills`.

### Option B — Project-local skill

If you want the skill scoped to a single project (e.g. shared with your team via the repo):

```bash
git clone https://github.com/MeveraStudios/Imperat-AI-Skill.git \
  .claude/skills/imperat-commands
```

Commit `.claude/skills/imperat-commands/` and every teammate using Claude Code in that repo gets the skill automatically.

### Option C — Manual download (no git)

1. Click **Code → Download ZIP** above.
2. Unzip and rename the folder to `imperat-commands`.
3. Move it into either:
   - `~/.claude/skills/` (global), or
   - `<your-project>/.claude/skills/` (project-local).

---

## ✅ Verify it loaded

In Claude Code, type:

```
/skills
```

You should see `imperat-commands` in the list. If not, double-check the folder path — Claude Code looks for skills under `.claude/skills/<name>/SKILL.md` (project) or `~/.claude/skills/<name>/SKILL.md` (global).

---

## 🎯 Try it out

Open Claude Code in any project that already declares an Imperat dependency, and ask things like:

| You say… | Claude does… |
|---|---|
| `Create a /heal command for Bukkit with a player argument and a permission node.` | Reads `First-Command/CreateYourFirstCommand.mdx`, `Arguments Masterclass/Supported-Types.mdx`, `Extras/@Permission.mdx`, `Platforms Guide/Bukkit.md`. Emits the full class + registration. |
| `Add a 5-second cooldown to my /report command.` | Reads `Extras/@Cooldown.mdx`, edits your existing class. |
| `Make a JDA slash command that mutes a user for N minutes.` | Reads `Platforms Guide/JDA.md` + `Extras/@Range.mdx`. Emits a Discord-flavoured slash command. |
| `Convert this nested @SubCommand mess into one @PathwayCommand.` | Reads `First-Command/@PathwayCommand.mdx` and rewrites your file in place. |

The skill auto-detects whether you're on **v3** or **v4** by reading your build file. No flag needed.

---

## 🧠 How it works

```
imperat-commands/
├── SKILL.md               ← entry point Claude reads first
└── references/
    ├── v3/
    │   ├── INDEX.md       ← table-of-contents + task→file routing
    │   ├── Introduction/
    │   ├── First-Command/
    │   ├── Arguments Masterclass/
    │   ├── Execution-Pipeline/
    │   ├── Advanced/
    │   ├── Extras/        ← @Permission, @Cooldown, @Async, …
    │   ├── Dependency-Injection/
    │   ├── Kotlin/
    │   └── Platforms Guide/  ← Bukkit, BungeeCord, Velocity, Minestom, JDA, Hytale, CLI
    └── v4/
        └── (same layout)
```

When the skill triggers, Claude:

1. **Detects the version** — greps `pom.xml` / `build.gradle*` for `studio.mevera:imperat-core:<version>`. Falls back to **v4** if nothing's found.
2. **Opens `INDEX.md`** for the matched version. The index groups every doc by category and lists short "if the user asks X, read Y" routes.
3. **Loads only the docs needed** — never all 50 — and writes the answer.
4. **Cites the docs it consulted** at the end of the reply, so you can verify or dig deeper.

This is **progressive disclosure**: the skill itself is tiny (one SKILL.md file), and the 100+ bundled docs only enter context when actually needed. Token cost stays low.

---

## ❓ FAQ

<details>
<summary><strong>Does this work outside Claude Code (e.g. Claude.ai, the API)?</strong></summary>

Yes — Skills work in any Claude harness that supports them (Claude Code, Claude Agent SDK, Claude.ai's skills feature). The install location differs per harness; consult [Anthropic's skills docs](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) for non-CC platforms.

</details>

<details>
<summary><strong>How do I update to the latest docs?</strong></summary>

```bash
cd ~/.claude/skills/imperat-commands
git pull
```

The bundled docs mirror [docs.mevera.studio/docs/Imperat](https://docs.mevera.studio/docs/Imperat) — a `git pull` here picks up any doc updates we've published.

</details>

<details>
<summary><strong>What if my project uses Imperat v2 or older?</strong></summary>

Not supported — the skill only ships v3 and v4 references. v3 is the current default; v4 is in active development.

</details>

<details>
<summary><strong>Can I force a specific version?</strong></summary>

Yes — just say so in your prompt: *"Create this command targeting Imperat v4"*. The explicit mention overrides auto-detection.

</details>

<details>
<summary><strong>Where does the skill look for the dependency?</strong></summary>

In order: `pom.xml`, `build.gradle`, `build.gradle.kts`. It looks for any artifact matching `studio.mevera:imperat-*:<version>` and parses the leading major number.

</details>

<details>
<summary><strong>Does the skill modify my files?</strong></summary>

Same as Claude Code's default behaviour — Claude proposes edits/files and you approve them. The skill itself doesn't bypass any of that.

</details>

---

## 🛠️ Contributing / updating bundled docs

The docs under `references/v<N>/` are mirrored from the [MeveraDocs](https://docs.mevera.studio) site. To refresh them:

```bash
# from MeveraDocs repo root:
cp -r docs/Imperat/v3/* /path/to/Imperat-AI-Skill/references/v3/
cp -r docs/Imperat/v4/* /path/to/Imperat-AI-Skill/references/v4/
```

Then commit + push. PRs welcome for SKILL.md improvements (clearer routing, better examples, additional task shortcuts in INDEX.md).

---

## 🔗 Related

- 📘 **Imperat docs:** https://docs.mevera.studio/docs/Imperat
- 💻 **Imperat source:** https://github.com/MeveraStudios/imperat
- 💬 **Discord:** https://discord.mevera.studio
- 📖 **Claude Skills:** https://docs.claude.com/en/docs/agents-and-tools/agent-skills

---

<p align="center">
  Built by <a href="https://github.com/MeveraStudios">Mevera Studios</a> · MIT licensed
</p>
