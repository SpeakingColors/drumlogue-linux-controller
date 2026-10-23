# Roadmap: drumlogue controller

Controller for the Korg drumlogue.

## Done
- [x] Plugin skeleton: parameter map from the manual, CC and NRPN send/receive, host automation, cat-ui look, CI on three systems
- [x] Release packaging: installers (Windows, macOS, Linux Flatpak extension), uninstallers and PDF manual built by the release workflow (test builds, not tested on real machines)

## Next
- [ ] Test every map entry on the real device and flip it to verified (`hw` column in map/params.csv)
- [ ] Test the VST3 in Bitwig and Studio One on Linux
- [ ] Two-CC controls: find out which CC is the coarse one on the device

- [ ] See all sound controls at once ("Show all parts" in the Sound page mockup), default one part at a time
- [ ] Real MIDI I/O behind sequencer, sound, FX and mixer pages

## Every cat plugin
- Draw through `catui::RenderGate` so meters and animations stop when the window is not in front (audio, automation and MIDI keep running)
- Settings page with MIDI filters on by default, selectable preset/backup folders, UI size reset to 100%
