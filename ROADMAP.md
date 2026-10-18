# Roadmap: drumlogue controller

Controller for the Korg drumlogue.

## Next
- [ ] See all sound controls at once ("Show all parts" in the Sound page mockup), default one part at a time
- [ ] Real MIDI I/O behind sequencer, sound, FX and mixer pages

## Every cat plugin
- Draw through `catui::RenderGate` so meters and animations stop when the window is not in front (audio, automation and MIDI keep running)
- Settings page with MIDI filters on by default, selectable preset/backup folders, UI size reset to 100%
