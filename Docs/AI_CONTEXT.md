# AI_CONTEXT.md — Primary Context Restoration

Read this file first. Then read TODO.md. Load other Docs/ files only as needed.

## Project
Fork of Equalizer APO (system-wide Windows audio APO / equalizer).
**Objective:** Add native VST3 plugin support while preserving full VST2 compatibility.

## Current Milestone
**M1 — VST3 SDK integration & build scaffolding** (Not Started)

## Active Objective
Vendor the Steinberg VST3 SDK (hosting portion only), add a static-lib project
to EqualizerAPO.sln, verify the whole solution still builds x86 + x64.

## Branch
`main` (create `feature/vst3` before M1 work begins)

## Blockers
- None technical. Note: repo is GPLv2-or-later; VST3 SDK is GPLv3 — combined
  work is distributed under GPLv3. Documented, acceptable.

## Important Files
| Area | Files |
|---|---|
| VST2 DLL loading | helpers/VSTPluginLibrary.{h,cpp} |
| VST2 instance/dispatcher | helpers/VSTPluginInstance.{h,cpp}, helpers/aeffectx.h |
| VST2 filter | filters/VSTPluginFilter.{h,cpp} |
| VST2 factory (config cmd `VSTPlugin`) | filters/VSTPluginFilterFactory.{h,cpp} |
| Factory registration | FilterEngine.cpp (~line 41-55, initialize list) |
| Editor GUI for VST2 | Editor/guis/VSTPluginFilterGUI*.{h,cpp} |
| Build | EqualizerAPO.sln, Common.vcxproj, build.bat |

## Next Task
M1: add `vendor/vst3sdk/` (hosting sources only: pluginterfaces/, public.sdk
hosting classes, base/), create `VST3Host.vcxproj` static lib, add to solution,
build clean in both archs. No Equalizer APO code changes yet.

## Architecture Summary (VST3 plan)
Mirror the VST2 layering exactly:
- `helpers/VST3PluginLibrary` — loads .vst3 module (Module/PluginFactory)
- `helpers/VST3PluginInstance` — IComponent + IAudioProcessor + IEditController lifecycle
- `filters/VST3PluginFilter` — IFilter impl, process via ProcessData
- `filters/VST3PluginFilterFactory` — new config command `VST3Plugin`, registered
  in FilterEngine.cpp alongside (not replacing) VSTPluginFilterFactory
- `Editor/guis/VST3PluginFilterGUI*` — later milestone

## Known Issues
- None yet (no VST3 code exists).

## Roadmap
M1 SDK+build scaffolding → M2 module loading (VST3PluginLibrary) →
M3 instance lifecycle (VST3PluginInstance) → M4 audio processing (filter + factory,
engine registration) → M5 state/params (chunk save, param map) → M6 Editor GUI →
M7 docs/polish/regression tests vs VST2. Details in TODO.md.
