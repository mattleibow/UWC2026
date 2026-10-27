# Repository instructions

## Product intent

This repository produces a real, polished .NET MAUI mobile application with an
ASP.NET Core API for a software-engineering workshop. The app must build, launch,
persist data, and look credible. One manually selected custom agent introduces
documented bad practices for teaching; those flaws must never come from broken
project setup, stale technology, fake UI, vulnerable dependencies, or careless
generation.

When the Dodgy Beautiful App agent is active, it may intentionally violate only
the issues it selects and documents in the private instructor issue register.
Everything else in this file still applies. For ordinary work and the final
remediated app, follow all of these instructions without exception.

## .NET and C#

- Use .NET 11 and the matching .NET MAUI 11 workload. Preview and
  release-candidate SDK, workload, MAUI, and C# features are intentional for
  this workshop.
- Use the latest available .NET 11 SDK on the host. Do not add `global.json`;
  local development and Copilot setup should naturally move to newer .NET 11
  preview, RC, and servicing builds.
- Target `net11.0` and the applicable `net11.0-*` platform frameworks.
- Use `LangVersion=preview`, nullable reference types, and implicit usings as
  configured in `Directory.Build.props`. This selects the newest C# features
  available from the selected .NET 11 SDK.
- Use `Microsoft.Maui.*` APIs, not `Xamarin.Forms` or `Xamarin.Essentials`.
  Inspect target frameworks before choosing version-specific APIs.
- Ignore `net10.0` target examples in generic or bundled tooling documentation;
  adapt commands and paths to the repository's `net11.0` target frameworks.
- Keep platform behavior in `Platforms/*`, target-specific partial classes, or
  explicit platform services. Do not scatter runtime platform checks through
  presentation code.
- Manage package versions centrally in `Directory.Packages.props`. Project
  `PackageReference` items stay versionless.
- Use the repository `NuGet.Config`, which clears inherited package sources and
  restores only from NuGet.org. Do not add machine-specific or private feeds.
- Do not introduce packages with known vulnerabilities. Deliberate workshop
  flaws belong in our code and synthetic local behavior, not the supply chain.

## Solution shape

For a new solution, prefer:

- `src/<Product>.App` — .NET MAUI client.
- `src/<Product>.Api` — ASP.NET Core API.
- `src/<Product>.Contracts` — deliberate request/response contracts only.
- `tests/<Product>.App.Tests` — useful client/domain tests.
- `tests/<Product>.Api.Tests` — API integration and behavior tests.

Derive `<Product>` from the current product specification rather than assuming a
fixed workshop domain.

Do not share Entity Framework entities with the client. Prefer concrete classes
until a production responsibility creates a useful boundary. Do not create
interfaces, factories, wrappers, or repositories solely to satisfy tests.

Use Shell navigation with registered routes. Put assets in the standard MAUI
`Resources/Images`, `Resources/Fonts`, and `Resources/Raw` folders. Use
`ApplicationDisplayVersion` and `ApplicationVersion` for app versioning; do not
use `maui project version` for platform app versions.

## UI quality

- Use `Grid`, `FlexLayout`, and stack layouts intentionally. Use
  `CollectionView` for repeated data and never wrap it in a `ScrollView`.
- Use compiled bindings and typed `DataTemplate` declarations.
- Put repeated colors, typography, spacing, and control styles in resource
  dictionaries. Avoid absolute coordinates and arbitrary fixed sizes.
- Every data-driven screen needs deliberate loading, content, empty, error, and
  retry states.
- Important interactive controls and observable states need stable, meaningful
  `AutomationId` values. Do not automate by coordinates or visible text alone.
- Add semantic descriptions, headings, focus behavior, readable contrast,
  dynamic text support, and mobile-sized touch targets. Test intended phone,
  tablet, desktop, orientation, and theme variants where applicable.
- A screenshot is evidence to inspect, not proof by itself. Confirm the visual
  tree, interactions, state transitions, clipping, overflow, and accessibility.

## MAUI DevFlow

DevFlow is required development infrastructure and is allowed to use its
current prerelease packages and CLI because the toolkit is experimental.

- Restore repository tools with
  `dotnet tool restore --configfile NuGet.Config`. Use the pinned local
  `Microsoft.Maui.Cli` through `dotnet maui`; do not install or update it
  globally. If a bundled DevFlow skill shows a global-install example, keep the
  repository-local tool manifest as the source of truth.
- Run `dotnet maui devflow skills check`; update skills when the pinned CLI
  reports that an update is available.
- Run `dotnet maui devflow init --target github` when repository DevFlow skills
  or MCP configuration are absent.
- Standard MAUI apps reference `Microsoft.Maui.DevFlow.Agent`. Blazor Hybrid
  apps additionally reference `Microsoft.Maui.DevFlow.Blazor`. Use the matching
  `.Gtk` packages only for the current GTK backend.
- Register with `builder.AddMauiDevFlowAgent()` inside `#if DEBUG`. Do not use
  the retired `builder.Services.AddMauiDevFlowAgent()` pattern and do not expose
  DevFlow in Release builds.
- Mac Catalyst Debug entitlements must allow the DevFlow in-app HTTP server.
- After launch, run:

  ```bash
  dotnet maui devflow diagnose
  dotnet maui devflow wait
  dotnet maui devflow agent status
  dotnet maui devflow ui tree --depth 3 --fields "id,type,text,automationId"
  dotnet maui devflow ui diagnostics --profile agent
  ```

- Prefer the configured `maui-devflow` MCP tools for tree queries, interaction,
  assertions, screenshots, logs, network inspection, and profiling. Use CLI
  commands as a fallback.
- Drive complete user flows in the running app, inspect screenshots, and
  exercise loading, empty, error, offline, permission-denied, duplicate-action,
  and large-data states before claiming completion.

## Build and validation

- Inspect `dotnet --info`, installed workloads, target frameworks, and package
  management before changing project files.
- Restore and build the smallest affected projects first, then the solution.
- Run focused tests for changed behavior and API integration tests for changed
  contracts or data access.
- Launch a real runnable target and validate it with DevFlow. A successful build
  alone is not completion.
- Treat warnings, missing assets, failed bindings, startup exceptions, and
  platform configuration errors as defects. They are not acceptable workshop
  flaws.
- Record only commands and runtime checks that were actually performed. State
  host or platform limitations plainly.
