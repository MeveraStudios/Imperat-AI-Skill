---
name: imperat-commands
description: Generate annotation-based commands using Imperat (the Mevera Studios command framework for Bukkit, BungeeCord, Velocity, Minestom, JDA, Hytale, and CLI). Use this skill whenever the user asks to create, register, or modify a command and either (a) explicitly mentions Imperat / `@Command` / `@Usage` / `@PathwayCommand` / the `studio.mevera:imperat-*` artifact, OR (b) the surrounding project already depends on Imperat (`imperat-core`, `imperat-bukkit`, etc.) and the user asks for a command, subcommand, slash command, or argument-parsing logic — even if they don't name the framework. Also trigger when the user asks "how do I add a permission/cooldown/argument/flag/subcommand" inside an Imperat project.
---

# Imperat Commands Skill

Imperat is an annotation-driven command framework. This skill produces correct, idiomatic Imperat command code by routing to the bundled v3 or v4 reference docs and reading only the pages relevant to the task.

## When to use

Use this skill when the user wants to **write or modify a command** in a project that uses Imperat. Common signals:

- Explicit: "create an Imperat command", "use `@Command`", `@PathwayCommand`, `@SubCommand`, `studio.mevera:imperat-core`.
- Implicit: project's `pom.xml` / `build.gradle` / `build.gradle.kts` declares an `imperat-*` dependency, and the user asks for a command (Minecraft slash/text command, JDA slash command, CLI command, etc.).
- Adding pieces to an existing Imperat command: arguments, flags, subcommands, permissions, cooldowns, custom argument types, exception handlers.

Do **not** use this skill for unrelated frameworks (Brigadier directly, Cloud, Lamp, ACF). If the project depends on a different command framework, mention Imperat as an option but defer to the user.

## Workflow

Follow these steps every time:

### 1. Detect the version (v3 or v4)

Imperat ships major versions with breaking API differences. Pick the right reference set:

1. Look for the dependency declaration in the user's project. Check, in order:
   - `pom.xml` — search for `<artifactId>imperat-core</artifactId>` and read the sibling `<version>`.
   - `build.gradle` / `build.gradle.kts` — search for `studio.mevera:imperat-core:<version>` (or any `imperat-*:<version>`).
2. Parse the leading major number:
   - `3.x.x` (or `3.x.x-SNAPSHOT`) → use `references/v3/`.
   - `4.x.x` (or `4.x.x-SNAPSHOT`) → use `references/v4/`.
3. If you cannot find a dependency declaration, **default to `references/v4/`** (current latest). Note this assumption in your reply so the user can correct you.
4. If the user explicitly names a version in the prompt ("for Imperat v3"), that wins over auto-detection.

### 2. Pick the right reference pages

Open `references/v<N>/INDEX.md` first. It groups every doc by category and lists which pages to read for common tasks ("create a basic command", "add a permission", "custom argument type", etc.). Read **only** the files INDEX points you at — there are 50 docs per version and loading all of them wastes context.

Always read the matching `Platforms Guide/<platform>.md` when the platform is known (Bukkit, BungeeCord, Velocity, Minestom, JDA, Hytale, CLI). Platform-specific registration code lives there.

### 3. Generate the code

Produce a complete, runnable command class (or Kotlin file when the project is Kotlin — check for `.kt` files or `kotlin` plugin in the build script). Match the user's existing package layout if visible.

A typical answer includes:

- The command class with `@Command`, `@Usage`, `@SubCommand`, `@PathwayCommand`, etc., as required.
- Argument annotations (`@Named`, `@Default`, `@Flag`, `@Switch`, `@Greedy`, `@Range`, `@Values`, `@Format`) where the user described typed parameters.
- Cross-cutting annotations (`@Permission`, `@Cooldown`, `@Async`) when the user mentioned them or they fit the request.
- A registration snippet that calls the platform-specific `Imperat` instance (e.g. `BukkitImperat.builder(this).build().registerCommand(new MyCommand())`). Pull the exact builder + registration call from the matching platform doc — APIs differ between v3 and v4 and between platforms.
- Required Maven/Gradle dependency lines only if the user has not yet added them. If they already have `imperat-core` declared, do not repeat it.

### 4. Cite which docs you used

End the reply with a short "References consulted" list pointing at the doc files you read (e.g. `First-Command/CreateYourFirstCommand.mdx`, `Platforms Guide/Bukkit.md`). This lets the user verify and dig deeper without round-tripping.

## Why this matters

Imperat's API surface is broad — commands, subcommands, pathways, custom argument types, processors, return resolvers, exception handlers, dependency injection. Generating code from memory or generic Java knowledge produces APIs that don't exist or that belong to other frameworks (Cloud, Lamp). Reading the bundled docs is the only reliable way to emit code that actually compiles against the user's Imperat version.

The v3 and v4 references differ in non-obvious ways (parameter types, registration, return resolver shape). Crossing them silently produces broken code, so version detection is the first step every time.

## File map

```
imperat-commands/
├── SKILL.md                     ← you are here
└── references/
    ├── v3/
    │   ├── INDEX.md             ← read first when version = v3
    │   ├── Introduction/
    │   ├── First-Command/
    │   ├── Arguments Masterclass/
    │   ├── Execution-Pipeline/
    │   ├── Advanced/
    │   ├── Extras/
    │   ├── Dependency-Injection/
    │   ├── Kotlin/
    │   ├── Comparison/
    │   └── Platforms Guide/
    └── v4/
        ├── INDEX.md             ← read first when version = v4
        └── ...same layout as v3
```

## Common task → file shortcuts

These shortcuts let you skip INDEX.md when the task is unambiguous. If in doubt, still open INDEX.md.

- **Brand new command, no arguments** → `First-Command/CreateYourFirstCommand.mdx` + `First-Command/RegisteringYourCommand.mdx` + `Platforms Guide/<platform>.md`.
- **Command with typed arguments** → add `Arguments Masterclass/Supported-Types.mdx` + `Arguments Masterclass/Argument-Variants.mdx`.
- **Subcommands / nested hierarchy** → add `First-Command/Subcommands.mdx` (or `First-Command/@PathwayCommand.mdx` for flattened pathways).
- **Permission / cooldown / async / range / values / format / secret / shortcut / parse-order** → read the matching `Extras/@<Annotation>.mdx`.
- **Custom argument type** → `Arguments Masterclass/Custom-Arguments.mdx` + `Arguments Masterclass/Validators.mdx` + `Arguments Masterclass/Suggestions.mdx`.
- **Custom exception / error handling** → `Execution-Pipeline/Exceptions Guide.mdx` + `Execution-Pipeline/Exception-Handlers.mdx`.
- **Tab completion / suggestions** → `Arguments Masterclass/Suggestions.mdx`.
- **Programmatic (non-annotation) command** → `Advanced/Command-Builders.mdx`.
- **Auto help command** → `Advanced/Auto Command Help.mdx`.
- **Custom annotations / placeholders** → `Advanced/Custom-Annotations.mdx` + `Advanced/Annotation Placeholders.mdx`.
- **Dependency injection (Guice/Spring/etc.)** → `Dependency-Injection/InstanceFactory.mdx`.
- **Kotlin DSL** → `Kotlin/Using-Kotlin.mdx`.
- **Configuration tweaks** → `Advanced/Configuring-Imperat.mdx`.

## Output style

- Default to Java unless the project is Kotlin or the user asks otherwise.
- Show the full command class, not snippets, so the user can paste it directly.
- Include imports.
- Use the user's existing package name when visible.
- Keep explanatory prose short — the code + a brief "what this does" sentence + the registration call is usually enough.
- Reproduce the **exact** annotation names from the docs; do not invent shortcuts.
