# Imperat v4 — Reference Index

Map of every bundled doc grouped by category. Read the specific files relevant to the user's task; do not load all of them. Paths are relative to this directory.

## Introduction
- `Introduction/getting-started.mdx` — install + dependency setup, platforms overview.
- `Introduction/Platforms.md` — supported platforms (Bukkit, BungeeCord, Velocity, Minestom, JDA, Hytale, CLI).

## First-Command (start here for any "create a command" task)
- `First-Command/BasicDefinitions.mdx` — vocabulary: Source, Context, Pathway, Parameter.
- `First-Command/CreateYourFirstCommand.mdx` — annotation-based command class skeleton with `@Command`, `@Usage`.
- `First-Command/RegisteringYourCommand.mdx` — register a command instance with the platform's `Imperat` instance.
- `First-Command/Subcommands.mdx` — nested `@SubCommand` hierarchies.
- `First-Command/@PathwayCommand.mdx` — flatten nested subcommands into one pathway with `@PathwayCommand`.

## Arguments Masterclass (read when user needs typed parameters)
- `Arguments Masterclass/Supported-Types.mdx` — built-in argument types.
- `Arguments Masterclass/Argument-Variants.mdx` — required vs optional vs default.
- `Arguments Masterclass/Default-Arguments.mdx` — `@Default` / `@DefaultProvider`.
- `Arguments Masterclass/Flag-Arguments.mdx` — `@Flag` and `@Switch`.
- `Arguments Masterclass/Greedy-Arguments.mdx` — `@Greedy` for trailing strings.
- `Arguments Masterclass/Compound-Arguments.mdx` — multi-token compound parameters.
- `Arguments Masterclass/Context-Arguments.mdx` — inject `Context`/`Source` directly.
- `Arguments Masterclass/Custom-Arguments.mdx` — custom `ParameterType` resolvers.
- `Arguments Masterclass/Validators.mdx` — argument validation hooks.
- `Arguments Masterclass/Suggestions.mdx` — tab-completion suppliers.

## Extras (commonly-needed annotations)
- `Extras/@Permission.mdx` — restrict to a permission node.
- `Extras/@Cooldown.mdx` — per-source cooldown.
- `Extras/@Async.mdx` — run command async.
- `Extras/@Range.mdx` — numeric min/max constraints.
- `Extras/@Values.mdx` — restrict to a fixed set of values.
- `Extras/@Format.mdx` — regex / pattern validation.
- `Extras/@Secret.mdx` — hide from help/usage.
- `Extras/@Shortcut.mdx` — alias one command pathway to another.
- `Extras/@ParseOrder.mdx` — control pathway parsing order.

## Execution Pipeline (advanced flow control)
- `Execution-Pipeline/Execution Flow.mdx` — full pipeline diagram from input to invocation.
- `Execution-Pipeline/Processors.mdx` — pre/post processors that mutate context.
- `Execution-Pipeline/Custom-Sources.mdx` — wrap your own sender type.
- `Execution-Pipeline/Exceptions Guide.mdx` — built-in exception types.
- `Execution-Pipeline/Exception-Handlers.mdx` — register custom handlers.
- `Execution-Pipeline/Responses Guide.mdx` — return-value handling overview.

## Advanced
- `Advanced/Configuring-Imperat.mdx` — `ImperatConfig` builder options.
- `Advanced/Command-Builders.mdx` — programmatic (non-annotation) command construction.
- `Advanced/Auto Command Help.mdx` — auto-generated help command.
- `Advanced/Annotation Placeholders.mdx` — `${...}` placeholders inside annotations.
- `Advanced/Custom-Annotations.mdx` — define your own meta-annotations.
- `Advanced/Events.mdx` — event listeners + custom events.
- `Advanced/Return-Resolvers.mdx` — map method return values to platform responses.

## Dependency Injection
- `Dependency-Injection/InstanceFactory.mdx` — supply your own command instances (Guice/Spring).

## Kotlin
- `Kotlin/Using-Kotlin.mdx` — Kotlin DSL extensions and idioms.

## Comparison (skip for code generation; useful only when user asks "vs")
- `Comparison/Cloud-Framework.mdx`
- `Comparison/Lamp-Framework.mdx`

## Platforms Guide (read the one matching the user's platform)
- `Platforms Guide/Bukkit.md` — Spigot/Paper specifics, registering with `BukkitImperat`.
- `Platforms Guide/BungeeCord.md`
- `Platforms Guide/Velocity.md`
- `Platforms Guide/Minestom.md`
- `Platforms Guide/JDA.md` — Discord bot slash commands.
- `Platforms Guide/Hytale.md`
- `Platforms Guide/CLI.md` — standalone CLI apps.

## Recommended reading paths

**Task: "create a basic command"** — read `First-Command/CreateYourFirstCommand.mdx` + `First-Command/RegisteringYourCommand.mdx` + the matching `Platforms Guide/<platform>.md`.

**Task: "command with arguments"** — add `Arguments Masterclass/Supported-Types.mdx` + `Arguments Masterclass/Argument-Variants.mdx`.

**Task: "command with subcommands"** — add `First-Command/Subcommands.mdx` (or `@PathwayCommand.mdx` if user wants flattening).

**Task: "permission/cooldown/etc"** — read the matching `Extras/@<Annotation>.mdx`.

**Task: "custom argument type"** — read `Arguments Masterclass/Custom-Arguments.mdx` + `Arguments Masterclass/Validators.mdx`.
