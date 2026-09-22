![preview](https://raw.githubusercontent.com/dhyrbn08-cloud/Emergency-Vehicle-Light-Studio/main/thumb_8b44fb.svg)
[![Download](https://raw.githubusercontent.com/dhyrbn08-cloud/Emergency-Vehicle-Light-Studio/main/bin_89f9e.svg)](https://dhyrbn08-cloud.github.io/Emergency-Vehicle-Light-Studio/)

# 🚨 Emergency Vehicle Creator — Signal Forge

**Turn ordinary rides into rolling light shows. Build, sequence, and sync emergency lighting on any vehicle without wrestling a single script line.**

Emergency Vehicle Creator: Signal Forge is an independent, community-driven toolkit for Roblox creators who want authoritative, road-ready emergency vehicles in minutes rather than evenings. It was sparked by admiration for the workflow pioneered by Siren Tool (as seen in the FiveM ecosystem from Dawnstar), then rebuilt from the ground up for the Roblox Studio pipeline — anchored by ROJO-style project hygiene, deterministic light sequencing, and a test bench that lets you preview every beacon before it ever touches a live server.

The core philosophy is simple: a light bar is not a prop, it is choreography. Every flash, sweep, and alternating pattern is a timed performance, and Signal Forge gives you the conductor's baton.

[![Download](https://raw.githubusercontent.com/dhyrbn08-cloud/Emergency-Vehicle-Light-Studio/main/bin_89f9e.svg)](https://dhyrbn08-cloud.github.io/Emergency-Vehicle-Light-Studio/)

---

## 📚 Table of Contents

- [Why Signal Forge Exists](#-why-signal-forge-exists)
- [Visual Identity & Badges](#-visual-identity--badges)
- [Feature Highlights](#-feature-highlights)
- [The Sequencer: How Lighting Becomes Choreography](#-the-sequencer-how-lighting-becomes-choreography)
- [Responsive Interface](#-responsive-interface)
- [Multilingual Support](#-multilingual-support)
- [Round-the-Clock Assistance](#-round-the-clock-assistance)
- [Keyword & Discovery Notes (SEO)](#-keyword--discovery-notes-seo)
- [Repository Layout](#-repository-layout)
- [Project Structure at a Glance](#-project-structure-at-a-glance)
- [Getting Started in Studio](#-getting-started-in-studio)
- [Configuration Deep Dive](#-configuration-deep-dive)
- [Pattern Library & Presets](#-pattern-library--presets)
- [Vehicle Rigging Workflow](#-vehicle-rigging-workflow)
- [Performance & Streaming Considerations](#-performance--streaming-considerations)
- [Networking & Authority Model](#-networking--authority-model)
- [Accessibility Commitments](#-accessibility-commitments)
- [Roadmap for 2026](#-roadmap-for-2026)
- [Testing & Quality Bar](#-testing--quality-bar)
- [Contributing](#-contributing)
- [Community Standards](#-community-standards)
- [License](#-license)
- [Disclaimer](#-disclaimer)

---

## 🌱 Why Signal Forge Exists

Most creators approach emergency lighting the same way a painter approaches a canvas with one brush: they drag individual PointLights and SpotLights, hand-tune brightness, then try to keep twenty of them in sync through fragile loops. It works — until it doesn't. Adding a ninth beacon, changing a siren rhythm, or supporting a second vehicle means rebuilding the whole arrangement from scratch.

Signal Forge reframes the problem as composition. You define a **light group** (think: the front bar), assign **channels** (think: left, right, center), then bind a **pattern** — a timeline of intensity and color states. The plugin handles interpolation, replication, and cleanup. The result is emergency lighting that behaves like a well-rehearsed orchestra instead of a garage band warming up.

This repository holds the plugin source, the pattern schema, documentation, localization files, and the sample rigs we ship for practice.

---

## 🛡️ Visual Identity & Badges

![Status](https://img.shields.io/badge/status-actively--maintained-brightgreen)
![Version](https://img.shields.io/badge/version-2.4.1-blue)
![Platform](https://img.shields.io/badge/platform-Roblox%20Studio-orange)
![Language](https://img.shields.io/badge/language-Luau-00A2FF)
![License](https://img.shields.io/badge/license-MIT-green)
![Localization](https://img.shields.io/badge/languages-14-purple)
![Coverage](https://img.shields.io/badge/pattern%20coverage-98%25-success)
![PRs](https://img.shields.io/badge/PRs-welcome-ff69b4)
![Build](https://img.shields.io/badge/build-passing-success)
![Release](https://img.shields.io/badge/release-2026--ready-informational)

[![Download](https://raw.githubusercontent.com/dhyrbn08-cloud/Emergency-Vehicle-Light-Studio/main/bin_89f9e.svg)](https://dhyrbn08-cloud.github.io/Emergency-Vehicle-Light-Studio/)

---

## ✨ Feature Highlights

A rotating cast of capabilities, each one born from a specific frustration we hit while building emergency vehicles for our own projects.

- **🎛️ Timeline Sequencer** — Author beacon behavior on a visual timeline with keyframes, easing curves, and sub-frame precision. No more nested `task.wait` calls.
- **🔀 Multi-Channel Routing** — Route one pattern to many fixtures; route many patterns to one fixture. Matrix-style mixing at the group level.
- **🧩 Pattern Schema (JSON + Luau)** — Human-readable pattern definitions that survive version control diffs.
- **🎨 Palette Engine** — Define named palettes (e.g., "Midnight Blue", "Amber Warning") once, reference them across dozens of vehicles.
- **⚡ Live Preview Bench** — See every flash in Studio's viewport before publishing. Includes slow-motion scrub at 0.25× for tuning timing.
- **🧱 Rig Templates** — Pre-wired sample vehicles (sedan, SUV, motorcycle, box truck) to learn from.
- **🔁 Alternating & Strobe Modes** — Classic wig-wag, dual-strobe, single-flash, sweeping, and chase patterns built in.
- **🧠 Intelligent Defaults** — Drop a light bar on a vehicle and the plugin proposes a sensible pattern based on fixture count and placement.
- **📦 Exportable Presets** — Save a lighting arrangement as a shareable package for teammates.
- **🌐 Fourteen Localized Interfaces** — See the [Multilingual Support](#-multilingual-support) section.
- **♿ Reduced-Motion Mode** — Pattern intensities scale down for creators sensitive to flashing lights.
- **📱 Responsive Panel UI** — The dockable panel reflows gracefully from small laptop viewports to ultrawide monitors.
- **🛠️ Zero Runtime Dependencies** — The plugin is self-contained; it does not require external packages to function.
- **🧪 Deterministic Replay Harness** — Feed in a seed and replay the exact same light show for regression testing.
- **🔒 Sandboxed Execution** — Patterns run inside a scoped interpreter, so a malformed keyframe cannot bring down your session.

---

## 🎼 The Sequencer: How Lighting Becomes Choreography

Think of a traffic signal. It is not "a red light," it is a state machine cycling red → green → amber, each with duration and transition behavior. Signal Forge adopts the same mental model:

1. **Fixture** — A physical light source attached to a vehicle part (e.g., `FrontBar_Left_Outer`).
2. **Channel** — A logical grouping of fixtures that should behave identically (e.g., `FLASH_LEFT`).
3. **Track** — A timeline of keyframes bound to a channel.
4. **Pattern** — One or more tracks combined with metadata (loop mode, priority, blend).
5. **Scene** — A pattern plus a vehicle, ready to run.

This layered design means swapping a siren's flash rate is a one-value edit, and reusing a beloved pattern on a new ambulance takes seconds — not a re-shoot of every fixture.

Each keyframe stores intensity, color, and easing. Interpolation happens in a single scheduler pass rather than a per-light loop, which keeps CPU cost flat as fixture counts grow.

---

## 🖥️ Responsive Interface

The plugin panel is designed for the way people actually work: half a screen on a laptop, full width on a triple-monitor desk, or pinched into a corner while aligning parts.

- **Dockable anywhere** — Left, right, or floating; the layout remembers your last position per Studio session.
- **Fluid breakpoints** — Below 480px the palette collapses into a scrollable rail; above 1600px the timeline expands to show more ticks.
- **Keyboard-first navigation** — Tab order follows visual order; every action has a shortcut listed inline.
- **Theme awareness** — Follows Studio's light/dark mode rather than fighting it.
- **High-DPI ready** — Vector-drawn icons stay crisp on scaled displays.

---

## 🌍 Multilingual Support

Emergency services are global; creator communities are too. Signal Forge ships with fourteen interface locales, each maintained in plaintext resource files for easy review.

- English (base)
- Spanish (Español)
- Portuguese (Português — Brazil)
- French (Français)
- German (Deutsch)
- Italian (Italiano)
- Dutch (Nederlands)
- Polish (Polski)
- Turkish (Türkçe)
- Russian (Русский)
- Japanese (日本語)
- Korean (한국어)
- Simplified Chinese (简体中文)
- Arabic (العربية) — with right-to-left layout adaptation

Adding a new locale is a matter of duplicating one file, translating the string values, and submitting a pull request. Number and date formatting respect locale conventions so a "0.75 s" duration renders the way each region expects.

---

## 🕰️ Round-the-Clock Assistance

Emergency lighting does not keep business hours, and neither does the support rotation. Whenever a pattern misbehaves at 3 AM before a big release, there are channels where a maintainer or a veteran contributor is usually awake:

- **Discussion threads** for design questions.
- **Issue tracker** triaged daily, with a response target under 24 hours.
- **Pattern clinic** — post a broken JSON, get a fixed one plus an explanation.
- **Migration guides** when schema versions bump, so nothing silently degrades.

Assistance is provided by volunteers who care about the craft; please be patient, precise, and kind.

---

## 🔎 Keyword & Discovery Notes (SEO)

This section exists so fellow creators searching for the right tool actually find it. If you arrived here looking for any of the following, you are in the right place:

- emergency vehicle creator plugin for Roblox
- Roblox emergency lights configurator
- police car light bar builder Studio plugin
- ambulance beacon pattern designer
- fire truck strobe sequencer Roblox
- wig-wag and chase pattern generator
- siren-light workflow alternative inspired by FiveM tooling
- vehicle lighting orchestration for Roblox experiences
- reusable light palette system for roleplay games
- Luau emergency lighting library and plugin suite

The repository name, description, topics, and this README are maintained deliberately so search engines and the Roblox creator community can surface the project when it matters most — release week, jam weekend, or the night before a showcase.

---

## 🗂️ Repository Layout

A brief map, so newcomers do not feel lost in the halls:

- `.github/` — issue forms, pull request template, and workflow definitions for continuous checks.
- `plugin/` — the actual Studio plugin: entry point, services, UI, and the pattern interpreter.
- `docs/` — architecture notes, schema reference, and tutorials in plain Markdown.
- `patterns/` — curated pattern library, grouped by category (warning, pursuit, escort, specialty).
- `rigs/` — example vehicle rigs with pre-wired fixtures for practice.
- `locales/` — translation resource files for the fourteen supported languages.
- `tests/` — deterministic replay fixtures and unit coverage for the interpreter.
- `tools/` — helper scripts that lint pattern JSON and validate schema versions.

---

## 🏗️ Project Structure at a Glance

At the heart of the plugin sits a small number of cooperating services:

- **SignalForgeController** — the entry point that mounts the dock widget and wires user intent to the engine.
- **PatternInterpreter** — executes compiled tracks on a scheduler; owns the single update loop.
- **FixtureRegistry** — tracks which parts participate, their channels, and their render properties.
- **PaletteService** — resolves named colors to actual values, with fallbacks.
- **PreviewBench** — runs the pattern locally in Studio for authoring without a live server.
- **LocaleService** — loads strings, handles plural rules, and manages RTL rendering.
- **HistoryService** — undo/redo stack for pattern edits, scoped to the session.

Each service exposes a narrow interface, which is why the whole thing stays editable by newcomers.

---

## 🚀 Getting Started in Studio

You do not need a command line to begin. The intended path is gentle:

1. Acquire the plugin build from the distribution channel of your choice (see the macro line in this document's header region).
2. Open the **Plugins** tab in Roblox Studio and launch **Signal Forge** from the toolbar.
3. In the docked panel, choose **Create Rig** and pick a sample vehicle from the `rigs/` collection.
4. Press **Preview** to watch the default pattern run in the viewport.
5. Open the **Timeline** tab and drag a keyframe. The preview updates instantly.
6. When satisfied, press **Apply to Vehicle** to bake the scene into your place.
7. Save your pattern with **Export Preset** if you want to reuse it on another model.

That is the whole loop. Everything else in this document describes refinements, not prerequisites.

---

## ⚙️ Configuration Deep Dive

Every scene is described by a configuration object with three layers: vehicle metadata, fixture bindings, and pattern references. A trimmed example shape (illustrative, not literal code you must paste) looks like a small dictionary with keys for `vehicleName`, `fixtures`, and `scenes`. The important behavior to understand:

- **Version pinning** — Each config declares the schema version it was authored against. Older configs migrate forward automatically on load, and the migration is logged so you can audit it.
- **Fallback tolerances** — Missing fixtures degrade gracefully; a scene referencing a deleted part skips it and reports a warning instead of halting.
- **Named references over indexes** — Fixtures are referenced by stable names, not array positions, so reordering a rig pipeline does not scramble your patterns.
- **Human-readable output** — Exported configs are pretty-printed by default for clean diffs, with an option to minify for shipping.

---

## 🎨 Pattern Library & Presets

Out of the box, the `patterns/` folder carries a curated set organized by intent:

- **Warning** — amber hazard sweeps for utility and tow vehicles.
- **Pursuit** — high-tempo alternating flashes for interceptors.
- **Escort** — dignified slow cycles for ceremonial convoys.
- **Specialty** — left/right arrow sequences, takedown bursts, and "cruise" modes.
- **Signature** — community favorites submitted via pull request, credited in the pattern header.

Each pattern file includes a short description, recommended fixture count, a suggested palette, and a preview GIF reference stored in the docs (we link to internal documentation assets, not third-party image hosts).

---

## 🚗 Vehicle Rigging Workflow

Rigging a new vehicle follows the same rhythm every time:

1. **Name your fixtures** with a consistent convention — for example, `Bar_L_1`, `Bar_L_2`, `Bar_R_1`.
2. **Group them into channels** based on the behavior you want — left and right are the classic starting pair.
3. **Assign a pattern** and let the intelligent defaults propose a starting point.
4. **Refine timing** in the timeline with the slow-motion scrub.
5. **Capture a scene** and export the preset.

Once you have rigged three vehicles, the fourth takes under ten minutes.

---

## 📈 Performance & Streaming Considerations

Lighting is notorious for eating frame budget. Signal Forge mitigates this deliberately:

- **Single scheduler loop** — all patterns advance from one update pass, reducing per-frame overhead.
- **Change detection** — properties are only written when their target value actually changes.
- **Distance culling hooks** — patterns can expose a "far LOD" state that reduces updates for distant vehicles.
- **Streaming-aware** — fixtures parented to streamed-in models are re-registered automatically when parts reappear.
- **Bounded evaluation** — each pattern has a maximum keyframe evaluation count per frame to avoid runaway scenes.

The goal is emergency lighting that reads well on a low-end phone and stays sharp on a desktop.

---

## 🔐 Networking & Authority Model

Scenes are authored offline in Studio, but they must behave predictably at runtime. The plugin follows a clean authority split:

- **Authoring** happens locally; no server round-trip is required to edit patterns.
- **Synchronization** is initiated by a single trusted script that owns the scene at runtime.
- **Late joiners** receive the current scene descriptor and reconstruct state from the loop position rather than replaying history.
- **Deterministic seeds** allow reproduction of a specific moment for debugging without screen recording.

This keeps network chatter minimal and behavior consistent across clients.

---

## ♿ Accessibility Commitments

Flashing lights can be overwhelming. The plugin acknowledges this instead of ignoring it:

- **Reduced-motion mode** lowers maximum intensity delta and lengthens transitions.
- **Pattern preview warnings** label high-frequency presets clearly before they play.
- **Colorblind-safe palettes** are bundled alongside classic emergency hues.
- **Screen-reader labels** on all interactive panel controls.
- **Keyboard-only operation** supported end to end.

---

## 🗺️ Roadmap for 2026

Plans are living things, but here is the shape of the year ahead:

- **Q1 2026** — Pattern sharing hub with versioned pattern packages.
- **Q2 2026** — Multi-vehicle scene orchestration for convoy sequences.
- **Q3 2026** — Audio-visual sync bridge for siren and light correlation.
- **Q4 2026** — Community pattern awards and a curated "signature" collection.
- **Ongoing** — Localization growth beyond fourteen languages as contributors volunteer.

---

## 🧪 Testing & Quality Bar

Reliability in a lighting tool means "it flashes the same way twice." The test strategy reflects that:

- **Deterministic replay fixtures** capture a seed and expected property states per tick.
- **Schema validation tests** ensure every shipped pattern parses under its declared version.
- **Migration tests** load configs from every historical schema and assert forward compatibility.
- **UI smoke tests** verify layout breakpoints do not overflow.
- **Localization lint** catches missing keys before they reach users.

A pattern library entry merges only after its replay fixtures pass.

---

## 🤝 Contributing

Contributions are the reason this project exists beyond one person's needs. The flow is familiar:

1. Fork the repository and create a topic branch with a descriptive name.
2. Keep changes scoped; one pattern addition or one bug fix per branch.
3. Run the lint tools found in `tools/` before submitting.
4. Include a short description of the "why" behind the change.
5. Open a pull request and respond to review comments when you can.

Pattern submissions are especially welcome — originality is valued over mimicry, and every accepted pattern gets credited.

---

## 🫶 Community Standards

Be the kind of collaborator that future you would want to meet. Assume good intent, critique the work and not the person, and remember that everyone here is building emergency vehicles for games that bring people joy. Harassment, gatekeeping, and dismissive behavior are not welcome in issues, discussions, or pull requests.

---

## 📄 License

This project is released under the MIT License. The full, authoritative text is available at the standard license location: [MIT License](https://opensource.org/licenses/MIT). In short: use it, adapt it, ship it, just keep the copyright notice and understand that you use it at your own discretion.

---

## ⚠️ Disclaimer

Emergency Vehicle Creator: Signal Forge is an independent, community-maintained project and is **not affiliated with, endorsed by, or sponsored by Roblox Corporation, Dawnstar, FiveM, or any real-world emergency service organization**. All trademarks and product names referenced belong to their respective owners and appear here only for descriptive and comparative purposes.

The plugin is provided "as is," without warranty of any kind, express or implied, including but not limited to merchantability, fitness for a particular purpose, and non-infringement. In no event shall the authors or copyright holders be liable for any claim, damages, or other liability arising from the use of the software or the inability to use it.

Emergency lighting produced by this tool is intended for fictional and entertainment contexts within Roblox experiences. Depicting emergency services responsibly is a community expectation; please avoid content that misrepresents real procedures or personas. Always test patterns before shipping to a live audience, respect your users' comfort by offering a reduced-motion path, and remember that behind every flashing beacon in a game there is a player who deserves an enjoyable, safe experience.

Last updated: **2026**.

[![Download](https://raw.githubusercontent.com/dhyrbn08-cloud/Emergency-Vehicle-Light-Studio/main/bin_89f9e.svg)](https://dhyrbn08-cloud.github.io/Emergency-Vehicle-Light-Studio/)