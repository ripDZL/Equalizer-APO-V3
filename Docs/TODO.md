# TODO.md

States: Not Started | In Progress | Blocked | Completed

## M1 — VST3 SDK integration & build scaffolding [Not Started]
- [ ] Create branch `feature/vst3`
- [ ] Vendor VST3 SDK hosting subset into `vendor/vst3sdk/`
      (pluginterfaces/, base/, public.sdk/source/vst/hosting/, module loading)
- [ ] Add `VST3Host.vcxproj` static library project (x86 + x64, /MT to match)
- [ ] Add project to EqualizerAPO.sln; verify full solution builds both archs
- [ ] Document SDK version + license note in PROJECT_MEMORY.md

## M2 — Module loading: helpers/VST3PluginLibrary [Not Started]
- [ ] Load .vst3 bundle/dll via VST3::Hosting::Module
- [ ] Enumerate classes from PluginFactory (kVstAudioEffectClass)
- [ ] Ref-counted shared library cache (mirror VSTPluginLibrary pattern)
- [ ] Default plugin path: %ProgramFiles%\Common Files\VST3
- [ ] Unit smoke test: load a known free VST3, list classes

## M3 — Instance lifecycle: helpers/VST3PluginInstance [Not Started]
- [ ] Create IComponent + IEditController, connect via IConnectionPoint
- [ ] setupProcessing (sample rate, block size, float32)
- [ ] Bus activation: main stereo/N-ch in+out, deactivate aux buses
- [ ] setActive / setProcessing ordering (match APO thread constraints)
- [ ] Speaker arrangement negotiation for APO channel counts

## M4 — Audio path: filters/VST3PluginFilter + Factory [Not Started]
- [ ] VST3PluginFilter : IFilter — process() fills ProcessData, non-interleaved float
- [ ] Handle channel count mismatch (same strategy as VST2 filter)
- [ ] VST3PluginFilterFactory — config command `VST3Plugin Library <path> ...`
- [ ] Register factory in FilterEngine.cpp (keep VSTPluginFilterFactory untouched)
- [ ] End-to-end test: config file loads VST3, audio passes, no RT allocation

## M5 — State & parameters [Not Started]
- [ ] Save/restore component+controller state (base64 chunk in config, like VST2)
- [ ] Named parameter overrides via IEditController normalized params
- [ ] Parameter changes queue (IParameterChanges) applied on process thread

## M6 — Editor GUI [Not Started]
- [ ] VST3PluginFilterGUI* in Editor/guis (mirror VST2 GUI classes)
- [ ] IPlugView hosting in dialog (HWND attach)
- [ ] FilterTable.cpp integration

## M7 — Regression, docs, polish [Not Started]
- [ ] VST2 regression pass (existing configs unchanged)
- [ ] Latency reporting (getLatencySamples → APO)
- [ ] Update Wiki/ config documentation
- [ ] Finalize all Docs/ files
