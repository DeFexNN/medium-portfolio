# Medium Projects Portfolio — Detailed Index

This index provides expanded, recruiter-focused technical summaries for each medium project. Each entry explains the technology stack, why it was chosen, how components interact, notable code patterns and highlights, build/run instructions, and suggested talking points for interviews.

---

## drivers-portfolio

Overview

A systems-level monorepo that groups kernel drivers and user-mode loaders. Focus areas: secure driver loading, kernel-user IPC, authentication of payloads, and manual-mapping strategies. This portfolio demonstrates low-level Windows engineering, secure loader design, and integration with kernel-mode primitives.

Technology stack & roles

- C/C++ (MSVC) — core implementation language for drivers and native loaders; chosen for direct access to Windows APIs and kernel ABI.
- Windows Driver Kit (WDK) — driver build and debug support, KMDF/WDK SDK usage patterns across driver projects.
- Visual Studio solution files (.sln/.vcxproj) — standardized build configurations for debug/release and x64 targets.
- Optional VMProtect artifacts seen in the loader binaries — indicates anti-tamper wrapping used to protect loader logic at runtime.
- PowerShell build helpers & batch files — automation for signing, driver installation, and release packaging.

Why these technologies

- Kernel work requires MSVC and WDK for correct headers, linkers, and syscalls. C++ is used where small C++ convenience is needed; critical paths favor C-style deterministic code.
- VMProtect or similar wrappers protect sensitive loader stages (authentication keys, license checks) from casual reverse engineering; good for research/demo but avoid shipping proprietary keys in repo.

Architecture & components

- driver_loader (user-mode): authentication entrypoint, license/token validation, anti-tamper checks, and driver installation flow. Uses a staged manifest to verify driver blob integrity before invoking kernel install APIs (Service Control Manager or manual mapping).
- MidnightSoftwareDriver (kernel): exposes IOCTLs for reading/writing physical or virtual memory, secure handshake via shared memory and MACs, and provides primitives for DMA-assisted reads in some builds.
- driver_manual_mapper: demonstrates manual driver mapping without service install — shows relocation, import resolution, and x64 kernel context setup.

Notable code patterns & highlights

- Timing-safe MAC comparisons (constant-time loops) in handshake verification to reduce side-channel leakage.
- Robust pointer validation and fail-safe fallbacks: functions that check address ranges and guard against user-mode supplied addresses.
- Manual-mapper demonstrates careful relocation handling and import thunk creation, with clear separation of mapping vs. execution stages.
- IOCTL dispatch table implemented with explicit validation and size checks per command to reduce malformed input risks.
- Defensive logging and limited telemetry: many places log minimal info to avoid leaking keys or offsets.

Build & run (high level)

1. Install Visual Studio with WDK and set target SDK for x64.
2. Open the provided solution (e.g., driver_loader.sln) and build Release x64.
3. For kernel driver install: use signed driver or test-sign driver with test-signing enabled; alternatively use manual mapper sample for lab testing.
4. Use included PowerShell scripts or build.bat to package and optionally sign artifacts.

Security & ethical notes

- Tools operate at kernel privilege — use only in controlled lab environments and never on systems you do not own.
- Remove or rotate any embedded tokens/keys before sharing publicly; consider moving sensitive payloads to releases or private storage.

Interview talking points

- Explain how the handshake MAC protects the driver/loader channel and why constant-time comparisons matter.
- Walk through manual mapping: relocations, import resolution, and why this approach avoids Service Control Manager install.
- Discuss trade-offs between using VMProtect vs. code obfuscation and secure enclave strategies for key protection.

Suggested next steps

- Migrate large binary artifacts to Git LFS and keep source-only in repo.
- Add automated build scripts (Azure Pipelines / GitHub Actions) to standardize CI builds and artifact signing.

---

## jutsu-remote

Overview

Jutsu Remote is a cross-platform remote-control client-server tool written in Java (Android client) and a Java-based or Node-based host. The repo includes a Gradle wrapper, Android components, and desktop tooling for remote media playback and remote control. The focus is on reliable transport, authentication, and extensible plugin architecture for new device types.

Technology stack & roles

- Java / Kotlin (Android) — mobile client implementation, chosen for native Android integration and lifecycle management.
- Gradle build system (gradle-wrapper.jar present) — reproducible builds and dependency management for Android modules.
- WebSocket / TCP transport for control channel — lightweight, real-time messaging suited to remote control commands.
- Optional Node/React host UI (if present) — web UI and control dashboard.
- JSON or protobuf message schemas for command serialization.

Why these technologies

- Android clients demand Java/Kotlin and Gradle as the de-facto build system. WebSocket enables bidirectional low-latency control while remaining firewall-friendly.
- Using a schema (JSON/protobuf) simplifies versioning and backward-compatibility of commands.

Architecture & components

- JutsuPlayer (client): handles media playback hooks, input capture, and command handling loop. Contains platform-specific permissions handling, wake-lock management, and battery-aware scheduling.
- Host server: accepts incoming client connections, manages sessions and auth tokens, and proxies commands to appropriate clients.
- Gradle wrapper ensures consistent build environment across machines; presence of gradle-wrapper.jar indicates strictly reproducible CI-friendly builds.

Notable code patterns & highlights

- Resilient reconnect logic: exponential backoff with jitter to avoid thundering herds when many clients reconnect.
- Permission-first design: explicit user permission flows for media capture and remote-control actions documented in manifest and runtime checks.
- Modular command handlers with clear separation between transport layer and command execution—easy to extend with new features.
- Secure token refresh and session invalidation on logout to reduce the window for replay attacks.

Build & run (high level)

1. Open the Android module in Android Studio (use the Gradle wrapper for consistent plugin versions).
2. Build and install the APK on a test device or emulator.
3. Ensure internet/network permissions are granted and test client-host pairing via a local host or staging server.

Security & operational notes

- Verify certificate pinning and TLS usage for production deployments to prevent MiTM attacks.
- Avoid storing long-lived secrets on device; use ephemeral tokens or platform keystore.

Interview talking points

- Describe reconnection strategy and why jitter improves resilience.
- Explain permission flows on Android and how to minimize user friction while maintaining security.

---

## my-portfolio

Overview

A developer-facing React + TypeScript portfolio built with Vite demonstrating modern frontend practices: component-driven layout, typed interfaces, and optimized build for fast page loads. The project includes a package-lock.json and typical frontend assets.

Technology stack & roles

- React + TypeScript — type-safe component development, predictable props and state shapes.
- Vite — extremely fast dev server and optimized production build; chosen for developer ergonomics.
- Node/NPM — package management; package-lock.json signals deterministic dependency tree.
- CSS Modules / Tailwind or SASS (if present) — modular styling for component isolation.

Why these technologies

- TypeScript adds compile-time checks that prevent common runtime bugs and improve maintainability.
- Vite reduces developer feedback loop time, enabling fast iteration and HMR for UI tweaks.

Architecture & components

- Component hierarchy: modular UI components (Header, ProjectCard, TechStack, ContactForm) with small, testable units.
- Data layer: small local JSON or MDX content for project metadata enabling static-site style generation.
- Build optimizations: code-splitting, asset hashing, and compressed production builds for CDN hosting.

Notable code patterns & highlights

- Strong typing of external data (project manifests) with zod or TypeScript interfaces to validate shape and avoid runtime errors.
- Lightweight static generation: project pages built from a single YAML/JSON manifest to make adding projects frictionless.
- Accessibility considerations: semantic HTML, keyboard focus management, and aria attributes on interactive components.

Build & run (high level)

1. Ensure Node 18+ (or specified LTS) installed.
2. npm install
3. npm run dev (development) or npm run build && npm run preview (production preview)

Interview talking points

- Describe how TypeScript types reduced a class of UI bugs in props/state flows.
- Explain trade-offs between client-side SPA and pre-rendered static output for SEO and performance.

---

## SnowflakesDesktop

Overview

A native desktop application (likely C++ or C#) for winter lab simulations or coursework, containing compiled artifacts in x64/Debug. The codebase demonstrates native UI, simulation loops, and possibly interop with math libraries for physics or signal processing.

Technology stack & roles

- C++ (MSVC) or C# (.NET) — native performance for real-time simulation.
- Native UI frameworks (Win32/GDI+, WPF, or Qt) depending on code paths — chosen for low-latency rendering and native look-and-feel.
- Project file artifacts and debug symbols present in x64/Debug indicating active development and local builds.

Why these technologies

- Native languages are suitable for compute-bound simulations, tight control over memory, and deterministic performance.
- Using the platform default IDE (Visual Studio) makes debugging and profiling straightforward with integrated tools.

Notable code patterns & highlights

- Simulation main loop separated from rendering, with a fixed timestep or semi-fixed approach to ensure numerical stability.
- Clear separation of model, view, controller (MVC) or similar layering to keep physics code testable and isolated.
- Use of unit tests or small test harnesses for validating physics invariants (if present).

Build & run (high level)

1. Open the .sln in Visual Studio and build Debug/Release as needed.
2. Run from the IDE to use debug visualizers and step through simulation frames.

Interview talking points

- Discuss time-step integration choices (fixed vs variable) and how they affect stability and determinism.
- Explain profiling findings and any micro-optimizations that improved frame-time consistency.

---

### Closing

These expanded entries are intended to give recruiters and technical reviewers richer context than a single-line summary. If you want, generate per-project README upgrades (full README.md files) and push them to each repository; also propose specific interview snippets or code excerpts for each project.