# Central Video Control — Project Plan

## Purpose
Adapt the open-source Churchfront Video macOS application for Central Baptist Church's video control workflows across five campuses, beginning with Connors Hill.

## Campus rollout
1. Connors Hill (initial build and validation)
2. McDougall
3. Southeast
4. Sherwood Park
5. Central

## Architectural principles
- Each campus must be able to operate locally on Sunday without relying on internet connectivity.
- Campus-specific hardware configuration must remain isolated (ATEM address, input labels, cameras, screens, routing, NDI sources, presets).
- Shared application code and centrally managed documentation/configuration conventions should reduce divergence between campuses.
- Avoid a remote control plane that can accidentally operate the wrong campus. Campus selection and connection identity must be explicit.
- Keep volunteer controls simple and separate from engineer/setup controls.
- Preserve upstream licensing notices and document modifications. Replace Churchfront branding with Central branding; do not use Churchfront trademarks as Central branding.

## Proposed implementation phases
### Phase 0 — Baseline and safety
- Preserve upstream `main` unchanged.
- Work on `central-development`.
- Record upstream behavior, dependencies, licensing, build steps, and safety interlocks.
- Establish a local test plan before connecting to live hardware.

### Phase 1 — Central identity and application naming
- Rebrand visible app name, icon, bundle identifier, config directory, helper process names, and documentation.
- Retain Apache-2.0 and font license notices.
- Decide migration behavior for any existing CueVideo configuration; never silently overwrite it.

### Phase 2 — Multi-campus configuration model
- Design a campus profile model with explicit campus identity and per-campus hardware settings.
- Keep profiles local to each campus installation initially.
- Add validation and clear active-campus indicators.
- Do not introduce network synchronization until conflict handling, permissions, and offline behavior are specified.

### Phase 3 — Connors Hill implementation
- Capture confirmed ATEM model/IP, camera models/IPs, input labels, screen/aux routes, NDI source names, and operator button needs.
- Build a Connors Hill profile from verified details only.
- Test using mock devices first, then conduct supervised read-only discovery, then controlled command testing.

### Phase 4 — Volunteer operator experience
- Simplify Sunday-facing controls and labels.
- Keep engineering/setup tools behind a deliberate mode or permission boundary.
- Provide clear connection status, live/preview state, and safe failure behavior.

### Phase 5 — Roll out remaining campuses
- Create and validate one profile at a time.
- Keep campus-specific settings separate.
- Document hardware changes and profile revisions.

## Confirmed campus computer hardware
- Connors Hill: Apple Mac mini M2 (user confirmed).
- Southeast: Apple Mac mini M2 (user confirmed).
- Sherwood Park: Apple Mac mini M2 (user confirmed).
- These are workstation model confirmations only; macOS versions, RAM/storage, and network details remain unverified.

## Known upstream baseline
- Native SwiftUI macOS app, target macOS 14+, Swift tools 5.9.
- ATEM control over UDP 9910.
- PTZ support includes Canon HTTP and VISCA over IP/TCP.
- NDI runtime loaded at runtime by the bridge.
- Original configuration is JSON under the user's Application Support directory.
- Original project includes operator and engineer interfaces, tests, and mock ATEM tooling.
- Existing upstream handoff notes describe a tested ATEM 2 M/E Constellation HD and three Canon cameras at another church setup. Those IPs and hardware assignments are not Central/Connors Hill configuration and must not be reused.

## Open questions to resolve before wiring Connors Hill
- Exact ATEM model and control IP at Connors Hill.
- Camera models, control protocols, IPs, and ATEM input assignments.
- Actual source labels and which sources volunteers need.
- Screen destinations and ATEM aux routing requirements.
- Which NDI feeds are available and where.
- Mac model/macOS version and NDI runtime installation.
- Desired Central app name and visual identity.

## Definition of done for Connors Hill pilot
- App builds and launches on the target Mac.
- Correct campus identity is visible and cannot be confused with another campus.
- Correct hardware is discovered and displayed.
- Read-only state agrees with the physical switcher before control testing.
- Each enabled action is tested under supervision with a rollback plan.
- Volunteer operator workflow is documented and tested by someone other than the developer.
- No other campus profile can be affected by a Connors Hill session.
