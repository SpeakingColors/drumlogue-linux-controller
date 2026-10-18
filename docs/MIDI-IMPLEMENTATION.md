# drumlogue: MIDI implementation

Source: Korg drumlogue Owner's Manual (E4, published 02/2025), Global MIDI pages (pp. 47 to 48) and the two MIDI Implementation Charts (pp. 60 and 61, version 1.01, 2022-05-16). Text extracted from the PDF; nothing inferred.

## Global MIDI settings

| Setting | Values |
|---|---|
| MIDI ROUTING > CH | 1 to 16 (single channel), or multi-channel ranges: 1-12, 2-13, ... 5-16, 6-1 ... 16-11 |
| MIDI ROUTING > ROUTE | USB+MIDI, USB |
| MIDI RX > PRG CHG, CC, TRANSP | Off, On (each) |
| MIDI TX > PRG CHG, CC, TRANSP | Off, On (each) |
| SYNC IN > UNIT, POLAR | 8TH or 16TH; RISE or FALL |

CC receive and transmit need RX CC / TX CC On. Program change needs PRG CHG On. Clock and start/stop are received only when Clock Source is AUTO (not INT). Transport needs TRANSP On.

Two implementations exist: **single channel** (factory default) and **multi channel** (one channel per part, chosen with the CH setting).

## Common to both

- Notes: received only (velocity 1 to 127); note off not used; aftertouch and pitch bend not used.
- Program change 0 to 127 (both ways). SysEx: yes, Korg exclusive plus Inquiry.
- Song position received; clock and commands both ways; active sensing both ways.
- All Notes Off (CC 123 to 127) received. CC 120 (All Sound Off) and 121 (Reset All Controllers) received only.

## Single channel (default)

Channel 1 to 16, notes 36 to 55 map to parts (pitch is not affected by note number):

| Note | Part |
|---|---|
| 36 | BD |
| 37 | RS |
| 39 | CP |
| 40 | SD |
| 42 | CH |
| 45 | LT |
| 46 | OH |
| 50 | HT |
| 52 | SP1 |
| 53 | SP2 |
| 55 | MULTI |

| CC | Function | CC | Function |
|---|---|---|---|
| 7, 39 | Master Volume MSB, LSB | 80, 81, 82 | OH Tune, Decay, Level |
| 12, 13, 14 | Delay Param 1, 2, 3 | 83 | Reverb Return |
| 16, 17, 18, 19 | User Synth Param 1 to 4 | 85, 86, 87, 89 | Reverb Param 1 to 4 |
| 20 to 24 | BD Tune, Drive, Attack, Decay, Level | 91 | Reverb Level |
| 25 to 29 | SD Tune, Snap.Lv, Snap.Dec, Decay, Level | 92 | Delay Level |
| 44, 45 | Delay Return, Delay Reverb Send | 102 to 105 | SP1 Tune, Attack, Decay, Level |
| 46 to 49 | LT Tune, Detune, Decay, Level | 106 to 109 | SP2 Tune, Attack, Decay, Level |
| 50 to 53 | HT Tune, Detune, Decay, Level | 110 to 115 | VPM Note, Index, Ratio, EG, Attack, Release |
| 54 | Multi Level | 116 to 119 | Noise Color, Attack, Release, Peak |
| 55, 56, 57 | RS Tune, Decay, Level | 120, 121 | All Sound Off, Reset All Controllers |
| 58, 59, 60 | CP Tune, Decay, Level | | |
| 61, 62, 63 | CH Tune, Decay, Level | | |

## Multi channel (alternate)

Each part gets its own channel (default 1 to 12; any continuous range can be set; the range may wrap around 16). Part 12 is the FX/master part. Note number 0 to 127; pitch is unaffected by note number except on the MULTI part. CC values are per channel, so the same CC number means different things on different channels. Signals are transmitted on the channel of the part.

| Part | CCs and functions |
|---|---|
| 1 to 11 | 7, 39: Level MSB, LSB; 10: Pan; 91: Reverb Send; 92: Delay Send |
| 1 BD | 20 to 26: Tune, Drive, Attack, Decay, Sweep, Sw.Time, Hold |
| 2 SD | 25 to 29: Tune, Snap.Lv, Snap.Dec, Decay, Tone |
| 3 LT | 46 to 51: Tune, Detune, Decay, Sweep, Attack, Drive |
| 4 HT | 50 to 55: Tune, Detune, Decay, Sweep, Attack, Drive |
| 5 CH | 61, 62, 63, 80, 81: Tune, Decay, Attack, Start, End |
| 6 OH | 80, 81, 82, 83, 85: Tune, Decay, Attack, Start, End |
| 7 RS | 55 to 58: Tune, Decay, Start, End |
| 8 CP | 58 to 62: Tune, Decay, Attack, Start, End |
| 9 SP1 | 102 to 106: Tune, Attack, Decay, Start, End |
| 10 SP2 | 106 to 110: Tune, Attack, Decay, Start, End |
| 11 MULTI | 108 to 115: VPM Noise, Idx Mod, Note, Index, Ratio, EG, Attack, Release |
| 11 MULTI (noise) | 116 to 119: Noise Color, Attack, Release, Peak |
| 11 MULTI (user synth) | 16 to 23: User Synth Param 1 to 8; 24 to 31: Param 9 to 16 |
| 12 | 7, 39: Master Volume MSB, LSB; 91: Reverb Level; 92: Delay Level |
| 12 Delay | 12 to 19: Delay Param 1 to 8; 20 to 27: Param 9 to 16; 44, 45: Delay Return, Delay Reverb Send |
| 12 Master FX | 46 to 53: Param 1 to 8; 54 to 61: Param 9 to 16 |
| 12 Reverb | 83: Reverb Return; 85, 86, 87, 89, 90, 102, 103, 104: Param 1 to 8; 105 to 112: Param 9 to 16 |
| 1 to 12 | 120, 121: All Sound Off, Reset All Controllers (receive only) |

Part 11 shares CC numbers 108 to 115 (VPM) with part 10 (106 to 110) and part 12 uses 16 to 31 ranges for its own FX; this works because channels differ. The plugin must always send on the part's channel.

## Open items

1. CC value ranges, MSB/LSB use for the 14-bit master volume (7 and 39), and FX param meanings per effect type are not in the charts.
2. SysEx layout: Korg exclusive format is not in the manual; the logue SDK covers units, not patch data **(check)**.
3. Internal swing is not reachable over MIDI per our earlier notes; the charts do not list it.
