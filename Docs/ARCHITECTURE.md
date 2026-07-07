# ARCHITECTURE.md

## Overall
Equalizer APO = Windows Audio Processing Object (COM, loaded into audiodg.exe)
+ config-file-driven FilterEngine + Qt Editor app.

## Audio Engine / DSP Pipeline
FilterEngine parses config files (parser/), builds filter chain from
IFilterFactory list, processes float non-interleaved blocks through IFilter
instances. Channel routing via ChannelHelper.

## Plugin Hosting
- VST2 (existing): see PROJECT_MEMORY.md "VST2 Implementation Notes".
- VST3 (planned): vendor/vst3sdk static lib; VST3PluginLibrary (module +
  factory enumeration) → VST3PluginInstance (IComponent/IAudioProcessor/
  IEditController, bus + speaker arrangement setup) → VST3PluginFilter
  (ProcessData per block) → VST3PluginFilterFactory (`VST3Plugin` command).

## Thread Model
APO process() runs on audio engine thread — no blocking, no allocation.
Instance creation/config parsing happens on control thread. VST3 setActive/
setProcessing transitions must respect this split.

## Memory Ownership
Filters owned by FilterEngine chain; plugin libraries shared via ref-counted
cache; VST3 objects via FUnknown ref counting (use IPtr<> wrappers, no raw release).

## Update whenever architecture changes.
