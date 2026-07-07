# PROJECT_MEMORY.md — Permanent Knowledge Base

## Goals
Add native VST3 hosting to Equalizer APO; keep VST2 fully working; long-term maintainability.

## Status
Pre-implementation. Docs bootstrapped 2026-07-07. Roadmap M1–M7 defined (see TODO.md).

## Repository Structure (relevant)
- Root: Common.vcxproj (shared lib code), EqualizerAPO.sln, FilterEngine.{h,cpp}, IFilter.h, IFilterFactory.h
- filters/ — all DSP filters + factories (VST2: VSTPluginFilter*)
- helpers/ — VSTPluginLibrary, VSTPluginInstance, aeffectx.h (VST2 ABI header), LogHelper, StringHelper, ChannelHelper
- parser/ — config file parsing
- Editor/ — Qt-based config editor incl. guis/VSTPluginFilterGUI*
- EqualizerAPO/ — the APO COM object itself
- libHybridConv-0.1.1/ — convolution
- build.bat, Setup/ — build & installer

## VST2 Implementation Notes (reference for VST3 port)
- VSTPluginLibrary: LoadLibrary on .dll, caches instances by path (static map,
  weak_ptr style ref counting), getDefaultPluginPath from registry/Program Files.
- VSTPluginInstance: raw AEffect* via "VSTPluginMain"; dispatcher opcodes for
  open/setSampleRate/setBlockSize/mains-changed; processReplacing with
  non-interleaved float** buffers; chunk state via effGetChunk/effSetChunk,
  base64-encoded into config; named params by scanning effGetParamName.
- VSTPluginFilter: implements IFilter; owns instance; handles channel mapping.
- VSTPluginFilterFactory: parses `VSTPlugin Library <path> ChunkData <b64> <param> <val>...`
  from splitQuoted parameters; registered in FilterEngine.cpp factory list.
- FilterEngine processing uses float, non-interleaved, fixed block sizes → maps
  cleanly to VST3 kSample32 / ProcessData.

## Planned VST3 Architecture
Parallel stack, zero changes to VST2 files:
helpers/VST3PluginLibrary → helpers/VST3PluginInstance → filters/VST3PluginFilter
→ filters/VST3PluginFilterFactory (command `VST3Plugin`) → registered in FilterEngine.cpp.
SDK vendored under vendor/vst3sdk as static lib VST3Host.vcxproj.

## Design Decisions
- 2026-07-07: New config command `VST3Plugin` instead of overloading `VSTPlugin`
  (explicit, no auto-detection ambiguity, VST2 configs untouched).
- 2026-07-07: Vendor SDK hosting subset rather than git submodule (SourceForge
  origin, offline builds, pinned version).

## Known Limitations / Risks
- License: repo is GPLv2-or-later; VST3 SDK GPLv3 → combined binary distributed
  under GPLv3. Acceptable per "or later" clause; note in release docs.
- APO runs in audiodg.exe context: UI-less service process; VST3 plugins that
  require a message loop or COM STA on init may misbehave — same class of
  problem VST2 support already has; test with headless-safe plugins first.
- Real-time constraints: no allocation in process(); IParameterChanges queues
  must be preallocated.

## Technical Debt
- None recorded yet.

## Future Ideas
- Plugin sandboxing/out-of-proc hosting (crash isolation from audiodg).
- CLAP support using the same layering.

## References
- VST3 SDK: https://github.com/steinbergmedia/vst3sdk (hosting samples: public.sdk/samples/vst-hosting)
- Upstream: https://sourceforge.net/p/equalizerapo/code/
