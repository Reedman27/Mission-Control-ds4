# MissionControl-DS4

**Custom lightbar colours, full button remapping and live config reload for Dualshock 4 controllers on a hacked Switch.**

A fork of [ndeadly/MissionControl](https://github.com/ndeadly/MissionControl), the Atmosphere sysmodule that lets
third-party controllers pair with the Switch over Bluetooth. This fork leaves everything MissionControl already does
alone and adds Dualshock 4 features on top, plus a small homebrew app to control them from the console itself.

> **AI notice:** the additions in this fork were written with AI assistance (Claude, by Anthropic) and have only been
> partly tested on hardware. Read [AI_NOTICE.md](AI_NOTICE.md) before installing.

## What you get

- **Any lightbar colour.** Set one colour for every player, or a different one per player number (1-8), as a hex value.
  Brightness is adjustable too.
- **Remap every DS4 input.** Face buttons, L1/R1, L2/R2, stick clicks, D-pad, Share, Options, PS, and the three
  touchpad zones (left, centre, right, pressed by clicking the pad) can each be sent to any Switch button, to several at
  once (combos such as `zl+l`), or disabled.
- **No reboot to change settings.** The sysmodule re-reads `missioncontrol.ini` about four times a second. Edit the
  file by hand or with the app and the lightbar and bindings follow within about a quarter of a second.
- **A Switch app to do it with.** *DS4 Controller Setup* has a Lightbar page (colour sliders, presets, per-player
  targets) and a Buttons page (a drawn DS4 you can tap, a grid of all 20 bindings, and a Switch button picker).

Not included: analog stick remapping, and settings that other controller types only read when they connect.

## Configuration

Add these to `/config/MissionControl/missioncontrol.ini` under `[misc]`. They are all optional and the defaults match
stock MissionControl.

```ini
[misc]
; Lightbar colour as RRGGBB hex. Applies to all players unless overridden
dualshock4_lightbar_colour=ff00aa
; Override for a specific player number (1-8)
dualshock4_lightbar_colour_1=00ffff
; Brightness 0-9. 9 shows a custom colour at full strength
dualshock4_lightbar_brightness=9

; Remap: dualshock4_map_<input>=<button>[+<button>...]
dualshock4_map_cross=a
dualshock4_map_circle=b
dualshock4_map_l2=zl+l
dualshock4_map_touchpad_centre=home
```

- **Inputs:** `cross circle square triangle l1 r1 l2 r2 l3 r3 share options ps dpad_up dpad_down dpad_left dpad_right touchpad_left touchpad_centre touchpad_right`
- **Buttons:** `a b x y l r zl zr plus minus home capture stickl stickr up down left right none`

Every key, with its default, is documented in [`mc_mitm/config.ini`](mc_mitm/config.ini). Changes apply to every
connected Dualshock 4. `[bluetooth] host_name` and `host_address` still need a reboot.

## The app

`ds4color-setup/` is the *DS4 Controller Setup* homebrew app. It edits only the keys it manages
(`dualshock4_lightbar_*` and `dualshock4_map_*`), keeps the rest of your ini intact, and backs the original up once to
`missioncontrol.ini.bak`. With **Live apply** on (the default) every change is written straight away, so dragging a
colour slider moves the real lightbar. `+` saves, `-` switches page, `B` quits. See
[`ds4color-setup/README.md`](ds4color-setup/README.md) for all controls.

## Install

There are no prebuilt releases yet, so build from source (the steps are in [FORK.md](FORK.md)):

1. Build the sysmodule. Back up `atmosphere/contents/010000000000bd00/` on your SD card, copy the new `mc_mitm.nsp`
   over `exefs.nsp` in that folder, and reboot.
2. Build the app and copy `ds4color.nro` to `/switch/ds4color/` on the SD card.
3. Connect a Dualshock 4, launch *DS4 Controller Setup* from the Homebrew Menu and change a colour.

If the console misbehaves after step 1, put the backed-up `exefs.nsp` back.

## Status

The sysmodule and the app both compile, and the patched sysmodule boots and works with a Dualshock 4 on real hardware.
Live colour and binding changes from the app are still being tested, and one unexplained input-delay hiccup has been
reported. Treat it as experimental.

## Credits and licence

- **MissionControl** is by [ndeadly](https://github.com/ndeadly) and contributors. All the Bluetooth and controller
  support this fork builds on is theirs. This fork is not affiliated with or endorsed by upstream, so please report
  problems with the DS4 additions here, not to the upstream project. Upstream's own README is kept as
  [`README.upstream.md`](README.upstream.md).
- This fork is based on upstream commit `71bbe2d` ("Update for 23.0.0"). Every change since then is in this repository's
  git history, starting from that commit.
- Licensed under the **GNU General Public License, version 2 only**, the same as MissionControl. See
  [LICENSE](LICENSE). Upstream's copyright notices are kept in the source files. The new app files carry their own
  `GPL-2.0-only` header. There is no warranty.
