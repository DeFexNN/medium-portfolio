# Medium Projects Portfolio — Detailed Index

[Home](#medium-projects-portfolio---detailed-index) • [Projects Overview](#projects-overview) • [Technical Summaries](#technical-summaries) • [Detailed Summaries](#detailed-summaries) • [Contact](#contact)


This index provides expanded, recruiter-focused technical summaries for each medium project. Each entry explains the technology stack, why it was chosen, how components interact, notable code patterns and highlights, build/run instructions, and suggested talking points for interviews.

---

## Projects Overview

- [drivers-portfolio](https://github.com/DeFexNN/drivers-portfolio) — Systems-level monorepo for kernel drivers, loaders, and mapping utilities.
- [jutsu-remote](https://github.com/DeFexNN/jutsu-remote) — Cross-platform remote-control client and host (Android + server).
- [my-portfolio](https://github.com/DeFexNN/my-portfolio) — React + TypeScript portfolio built with Vite showcasing projects and component-driven design.
- [SnowflakesDesktop](https://github.com/DeFexNN/SnowflakesDesktop) — Native desktop simulation/coursework project with debug artifacts.


---

## Technical Summaries

Quick pointers to each project's technology highlights and why they matter for recruiters and reviewers.

- drivers-portfolio: Kernel/user-mode tooling demonstrating secure loader strategies, IOCTL-based IPC, and manual-mapping techniques for kernel payloads.
- jutsu-remote: Android client + host server showing resilient transport, permission-aware design, and modular command handling.
- my-portfolio: Modern React + TypeScript static-driven portfolio optimized for fast iteration and SEO-friendly pages.
- SnowflakesDesktop: Native desktop simulation demonstrating deterministic simulation loop design and profiling-ready builds.


---

## Detailed Summaries

### drivers-portfolio

**Repository:** https://github.com/DeFexNN/drivers-portfolio

**Overview**

A systems-level monorepo that groups kernel drivers and user-mode loaders. Focus areas: secure driver loading, kernel-user IPC, authentication of payloads, and manual-mapping strategies. This portfolio demonstrates low-level Windows engineering, secure loader design, and integration with kernel-mode primitives.

**Technology stack & roles**

- C/C++ (MSVC): core implementation language for drivers and native loaders; chosen for low-level Windows API access and direct memory control.
- Windows Driver Kit (WDK): driver compilation, KMDF/WDK samples, and correct kernel headers/linker behavior.
- Visual Studio solutions (.sln/.vcxproj): standard debug/release configurations with signing and packaging steps.
- VMProtect (observed in binary artifacts): anti-tamper layer applied to loader binaries in releases.
- PowerShell/batch scripts: automation for signing, packaging and driver test-install.

**Why these choices matter**

Kernel development requires MSVC + WDK to ensure ABI correctness and access to driver entry points. Using plain C/C++ keeps the runtime minimal and deterministic, which is crucial in kernel context where dynamic allocators or exceptions are discouraged.

**Code highlights & interesting patterns**

- Timing-safe MAC comparisons used in handshake verification to mitigate timing side-channels.
- Strong pointer validation routines before any kernel memory access to avoid faults and BSODs.
- Manual mapping implementation separating mapping, relocation fixups and import thunk resolution — useful talking point to explain loader internals.
- IOCTL handlers with explicit size and bounds checks to defend against malformed user-mode inputs.

**Build & test (high level)**

1. Install Visual Studio with WDK for target OS.
2. Open the solution and build x64 Release.
3. Use test-signing or a manual mapper for lab experiments. Follow included PowerShell scripts for packaging.

**Interview talking points**

- Explain how manual mapping avoids SCM and why relocation/import handling is non-trivial.
- Discuss MAC-based handshake design and constant-time compare rationale.


---

### jutsu-remote

**Repository:** https://github.com/DeFexNN/jutsu-remote

**Overview**

Cross-platform remote-control system with an Android client and host components. Emphasizes resilient transport, session/auth management and modular command execution for media and device control.

**Technology stack & roles**

- Java / Kotlin (Android): client implementation; chosen for native lifecycle and permission handling.
- Gradle (gradle-wrapper.jar): reproducible builds and CI-friendly workflow.
- WebSocket / TCP transports: low-latency bidirectional messaging for control commands.
- JSON / protobuf schemas: stable command schemas that enable backward compatibility.

**Code highlights & interesting patterns**

- Exponential backoff + jitter for reconnection to avoid synchronized reconnect storms.
- Permission-first user flows: runtime checking, graceful degrade when restricted, and clear user prompts.
- Decoupled transport and command handler layers for easy extension.

**Build & test (high level)**

1. Open the Android module in Android Studio (use the Gradle wrapper).
2. Build and install the APK on a device/emulator.
3. Run the host server locally to test pairing and command flows.

**Interview talking points**

- Describe session lifecycle and token refresh strategies used to limit attack windows.
- Explain reconnection strategy and how jitter improves reliability under load.


---

### my-portfolio

**Repository:** https://github.com/DeFexNN/my-portfolio

**Overview**

Developer portfolio built with React + TypeScript and Vite. Focus on typed component design, fast dev loop and static-driven pages for SEO.

**Technology stack & roles**

- React + TypeScript: type-safe UI components and predictable props/state flows.
- Vite: fast dev server and optimized production bundling.
- Node/NPM: dependency management; package-lock.json ensures deterministic installs.
- CSS Modules / Tailwind (if present): maintainable styling patterns.

**Code highlights & interesting patterns**

- Strong typing of external project manifests using TypeScript or zod to avoid runtime shape errors.
- Static generation of project pages from a single manifest for easy additions.
- Accessibility-first practices: semantic HTML, ARIA attributes and keyboard navigation.

**Build & test (high level)**

1. Node 18+ recommended.
2. npm ci && npm run dev for local dev; npm run build for production.

**Interview talking points**

- How TypeScript prevented a class of UI bugs and improved developer DX.
- Trade-offs between SPA and pre-rendered pages for SEO and initial load.


---

### SnowflakesDesktop

**Repository:** https://github.com/DeFexNN/SnowflakesDesktop

**Overview**

Native desktop application (coursework) demonstrating simulation loops, native UI and debug builds. Useful to show systems-thinking in UI and real-time code.

**Technology stack & roles**

- C++ (MSVC) or C# (.NET): native compute paths for deterministic simulations.
- Visual Studio solutions: integrated debugging, profiling and symbol handling.
- Native UI frameworks (Win32/GDI+, WPF, or Qt): chosen based on desired UX and render performance.

**Code highlights & interesting patterns**

- Separation of simulation update and rendering with fixed timestep or semi-fixed approach for numerical stability.
- Model-view separation that keeps physics/testable and isolated from UI.
- Use of debug harnesses and unit tests to validate invariants (when present).

**Build & test (high level)**

1. Open the .sln in Visual Studio and build Debug/Release.
2. Run under debugger for profiling and visual analysis.

**Interview talking points**

- Choice of integration timestep and effect on determinism and stability.
- Profiling results and micro-optimizations to improve frame-time consistency.


---

## Contact

- GitHub: https://github.com/DeFexNN

If you want, I can add a rendered TOC with anchors and clean spacing in each section or open PRs to apply per-repo README upgrades instead of pushing to main.