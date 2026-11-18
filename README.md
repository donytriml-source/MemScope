# MemScope

**Memory intelligence, engineered.**

MemScope is a precision memory profiler for Windows processes, written entirely in C# on .NET 8 / WPF.
It reads a process through documented Windows APIs — no injection, no profiler DLL, no guesswork — and turns the result into an engraved, self-contained dossier: an inclusive call tree, leak heuristics, squarified heap composition, hotspots, source reconstruction, live telemetry and branded exports.

Designed and engineered by **MR8AD AL-Tmimi**.

![MemScope home dossier](docs/screenshots/home.png)

---

## Why it feels different

- **One instrument, not a pile of graphs.** Dossiers, trees, treemaps, timelines and inspectors share a single visual language: Obsidian & Brass, hairline geometry, tabular numerals and restrained motion.
- **Answers before charts.** The analysis pass writes plain-language observations — what grew, what leaks, what churns, what is overhead — before you touch a filter.
- **Nothing hides behind a license server.** No NuGet dependencies, no telemetry, no network calls. A capture is a file you own.

## What it does

### Capture dossier
Every capture opens on a dossier: identity, target, duration, snapshot count, live heap, peak, allocation volume, backtrace count, trend, observations, heap composition, timeline and recent captures.

### Inclusive call tree
Every allocation path aggregated inclusively, ranked by live bytes, with module, share, allocations, frees and source location. Filter by text, module or minimum size; group by module; expand or collapse the whole tree.

### Leak heuristics
Backtraces are scored on retention, free ratio, churn, snapshot growth and block pressure. Each candidate arrives with reasons, confidence, file and line — not just a red number.

![Leak candidates](docs/screenshots/leaks.png)

### Hotspots
The busiest backtraces ranked across live bytes, allocation count, churn and retention, with a weighted score and a float-over detail strip.

![Hotspots](docs/screenshots/hotspots.png)

### Squarified treemap
A Bruls–Huizing–van Wijk treemap of the live heap. Hover for composition, click a tile to focus its backtrace in the inspector.

![Heap treemap](docs/screenshots/treemap.png)

### Live observatory
Attach to any process to sample working set, private bytes, pagefile, native heaps, modules, committed regions, thread and handle counts, and CPU. Choose 200 ms – 1 s sampling and toggle deep heap/region scans.

![Live monitor](docs/screenshots/live.png)

### Snapshot comparison
Compare the current capture against the built-in earlier Aurora capture or any other `.msc` and see which canonical backtraces grew, shrank, appeared or resolved.

![Capture comparison](docs/screenshots/compare.png)

### Source reconstruction and exports
The inspector reconstructs the leaf frame's source when the file is on disk, and captures can be exported as branded HTML, call-tree CSV or metrics JSON.

## Quick start

Requirements: Windows 10/11 and the .NET 8 SDK (or newer SDK with the .NET 8 Windows Desktop runtime).

```powershell
# Build everything
dotnet build MemScope.sln -c Release

# Launch the app
dotnet run --project src\MemScope.App -c Release

# Run the verification harness (51 assertions)
dotnet run --project src\MemScope.Tests -c Release

# Launch the deliberately leaky demo workload
dotnet run --project src\MemScope.Demo -c Release
```

MemScope opens on the synthetic **Aurora Studio** walkthrough, so every screen is populated before you attach to anything. `samples/aurora-demo.msc` is written by the test harness for a portable capture to open, compare and export.

## Command line

| Switch | Purpose |
| --- | --- |
| `--no-splash` | Skip the splash window (used by automated screenshots) |
| `--page home\|live\|analysis` | Open on a specific workspace |
| `--tab 0..4` | Open a specific analysis tab (call tree, hotspots, leaks, treemap, compare) |
| `--attach <name\|pid>` | Open Live and attach immediately to a process |
| `--compare` | Diff the loaded capture against the Aurora walkthrough baseline |
| `--accent Brass\|Emerald\|Violet\|Cyan\|Rose` | Preview a palette for this session only |
| `--shot <path>` | Render the main window to PNG and exit |
| `--shot-delay <ms>` | Wait before the screenshot (default 1400 ms) |
| `--show about\|splash` | Capture the About or splash surface instead of the shell |

## Keyboard

| Shortcut | Action |
| --- | --- |
| `Ctrl+K` | Command palette |
| `Ctrl+O` / `Ctrl+S` | Open / save a capture |
| `Ctrl+1` / `Ctrl+2` / `Ctrl+3` | Home / Live / Analysis |
| `Ctrl+D` | Replay the walkthrough |
| `Ctrl+E` | Export branded HTML |
| `Alt+R` | Rescan processes |
| `F1` | About |
| `F5` | Refresh processes (Live) |
| `Esc` | Close palette, inspector or dialog |

## Capture format

A `.msc` file is gzip-compressed UTF-8 JSON. The schema is intentionally boring: identity and metadata, modules, frames, aggregated stacks, allocation events in columnar form, snapshots, heaps and optional source files. See [docs/FORMAT.md](docs/FORMAT.md).

A JSON capture written without compression opens the same way — MemScope sniffs gzip before decoding.

## Solution layout

| Project | Responsibility |
| --- | --- |
| `src/MemScope.Core` | Models, capture format, analysis (call tree, leaks, hotspots, treemap, timeline, histogram, comparison), exports, native sampling, synthetic walkthrough |
| `src/MemScope.App` | WPF shell: custom chrome, rail navigation, dossier, analysis workspace, live observatory, charts, treemap, source viewer, settings |
| `src/MemScope.Demo` | A deliberately leaky render-loop workload for live-attach testing |
| `src/MemScope.Tests` | Console verification harness — 51 assertions over capture, analysis, comparison, exports and branding |

The UI has no NuGet dependencies and no third-party charting library; every control is drawn in WPF.

## Design system

- **Obsidian & Brass** — near-black obsidian surfaces, brass hairline accents, tabular numerals.
- Five runtime palettes: Obsidian brass, Emerald ink, Iris, Glacier and Ember. Accent resources are dynamic, so switching a palette changes buttons, bars, gradients, glows and selection states live.
- Custom chrome, 92 px icon rail, command palette, toast stack, splash and About dossier.
- Settings live at `%AppData%\MemScope\settings.json`.

<p align="center">
  <img src="docs/screenshots/splash.png" width="720" alt="MemScope splash" />
</p>

## Privacy and safety

- Live sampling uses `OpenProcess`, toolhelp heap/module snapshots and `VirtualQueryEx`-style region enumeration. It never writes to the target and never injects code.
- Captures are local files. MemScope performs no network I/O.
- Protected processes may require elevation for heap and region enumeration; the Live workspace says so explicitly instead of failing silently.

## Credits

**MemScope 1.0.0 · Obsidian · Release**
Crafted by **MR8AD AL-Tmimi** — مرقاد التميمي.

<p align="center">
  <img src="docs/screenshots/about.png" width="620" alt="The MemScope About dossier" />
</p>

Proprietary. All rights reserved. See [LICENSE](LICENSE).
