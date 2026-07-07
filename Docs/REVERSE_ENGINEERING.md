# REVERSE_ENGINEERING.md — Equalizer APO Internals Reference

(Developer reference manual — grow continuously.)

## FilterEngine.cpp
- Registers all IFilterFactory instances in initialize (includes at lines ~41-55).
- VST3 factory registration goes here (M4).

## Config parser
- Command word + parameters; factories claim commands (see
  VSTPluginFilterFactory::createFilter checking `command == L"VSTPlugin"`).
- Quoted parameter splitting via StringHelper::splitQuoted.

## VST2 host details
- helpers/aeffectx.h: bundled VST2 ABI definitions (Steinberg header re-creation).
- VSTPluginInstance::prepareForProcessing(sampleRate, blockSize),
  startProcessing(), processReplacing(float**, float**, int).

## To document as discovered
- APO registration/registry usage (DeviceAPOInfo.cpp)
- COM lifecycle of EqualizerAPO/ object
- Editor <-> config interaction
