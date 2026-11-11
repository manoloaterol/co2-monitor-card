# CO2 Monitor Card

A Home Assistant card that shows the screens of the CO2 monitor: the same
layout, typefaces and color scale as the device, with its history chart.

The three screens rotate like a carousel. Tap the card or swipe to move to the
next one; the dots below jump straight to a screen.

| Screen | What it shows |
|---|---|
| `big` | CO2 as large as it goes, framed in the color of its level |
| `co2` | CO2 and a tall history chart |
| `co2_env` | CO2, temperature, humidity and a shorter chart |

The chart is rebuilt from Home Assistant's history, so it survives a restart
of the monitor.

## Installation

### HACS

[HACS](https://hacs.xyz) does not come with Home Assistant. If you do not have
it, install it first with the [official guide](https://hacs.xyz/docs/use/), or
use the manual installation below.

1. In HACS, open the menu (⋮) and choose **Custom repositories**.
2. Add `https://github.com/manoloaterol/co2-monitor-card` with the type **Dashboard**.
3. Find **CO2 Monitor Card** in HACS and download it.
4. Reload the browser.

### Manual

1. Copy `co2-monitor-card.js` to the `www` folder of your Home Assistant
   configuration (create it if needed, then restart Home Assistant).
2. In **Settings → Dashboards**, open the menu (⋮), choose **Resources** and add
   `/local/co2-monitor-card.js` as a **JavaScript module**.
3. Reload the browser.

## Configuration

Add the card from the dashboard editor: search for **CO2 Monitor**. It picks
the first CO2 sensor it finds, and every option can be set from the visual
editor. In YAML:

```yaml
type: custom:co2-monitor-card
entity: sensor.living_room_co2
```

| Option | Default | Description |
|---|---|---|
| `entity` | (required) | The monitor's CO2 sensor. |
| `temperature_entity` | found on its own | Temperature sensor. Left empty, the card uses the one of the same device. |
| `humidity_entity` | found on its own | Humidity sensor, found in the same way. |
| `screens` | all three | Which screens to show: `big`, `co2`, `co2_env`. They keep the monitor's order. |
| `rotate_seconds` | `10` | Seconds per screen. `0` stops the carousel. |
| `chart_bars` | `40` | Bars in the chart: 20, 25, 40, 50 or 100. |
| `chart_bar_seconds` | `auto` | Seconds per bar: `auto`, 20, 30, 60, 120 or 300. |

`auto` takes the seconds per bar from the monitor, which reports them as the
`chart_bar_s` attribute of its CO2 sensor (firmware 0.10 and later). The
monitor publishes one reading per bar, so this is what makes the card's chart
match the display. With older firmware, set it to the value on the monitor's
settings page; the card falls back to 20 otherwise.

The number of bars is the card's own choice: more bars than the display shows
a longer stretch of history.

A single screen, with no carousel:

```yaml
type: custom:co2-monitor-card
entity: sensor.living_room_co2
screens: [big]
```

## Development

`co2-monitor-card.js` is the whole card: no build step and no dependencies.
Open `test/index.html` in a browser to see it against a stand-in for Home
Assistant; `?ppm=2600` tries another level.

The text is drawn as SVG paths taken from the fonts, so the card needs no web
font. `tools/gen_glyphs.py` regenerates them (it needs fontTools):

```sh
python3 tools/gen_glyphs.py <folder with Goldman-Bold.ttf, Goldman-Regular.ttf and RussoOne-Regular.ttf>
```

## Licence

The card is under the MIT licence (see `LICENSE`). The glyph outlines come from
Goldman and Russo One, under the SIL Open Font License 1.1 (see `fonts/`).
