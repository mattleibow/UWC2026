# Dodgy but Beautiful

Workshop materials for demonstrating that AI-generated software can look polished,
work on the happy path, and still contain serious engineering problems.

The repository contains:

- [`dodgy-beautiful-app`](.github/agents/dodgy-beautiful-app.agent.md), a
  reusable, manually selected GitHub Copilot custom agent that builds a polished
  app, selects coherent teaching flaws, and generates its private instructor
  pack.
- [The app specification](docs/app-spec.md), the student-facing product the
  team intends to build, with a copyable kickoff prompt at the top.
- [Repository Copilot instructions](.github/copilot-instructions.md), which keep
  both the deliberately flawed and remediated apps current, buildable, and
  genuinely polished.

The custom agent contains the reusable workshop curriculum. After it builds the
app, it generates the app-specific issue register, remediation flow, clean
architecture target, and validation report under `instructor/`.

The custom agent integrates MAUI DevFlow for runtime automation, visual-tree
inspection, layout diagnostics, screenshots, network/log inspection, and
end-to-end validation of the real running app.

Developer tools are pinned in [`dotnet-tools.json`](dotnet-tools.json). Restore
them with `dotnet tool restore --configfile NuGet.Config` and invoke the MAUI
CLI as `dotnet maui`. The same manifest pins `dotnet-counters`,
`dotnet-gcdump`, `dotnet-dump`, and `dotnet-trace` for evidence-based leak and
performance investigations.
[`NuGet.Config`](NuGet.Config) clears inherited user and machine feeds so
restores use only NuGet.org.

The project uses the latest available .NET 11 SDK and matching MAUI 11 workload.
[`Directory.Build.props`](Directory.Build.props) enables the latest preview C#
language version available from that SDK.

The generated application is intentionally unsafe for production. Run it only
with synthetic data in an isolated local demonstration environment.