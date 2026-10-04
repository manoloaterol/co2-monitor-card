# CO2 Monitor Card

A Home Assistant card that shows the screens of the CO2 monitor: the same
layout, typefaces and color scale as the device, with its history chart.

The three screens rotate like a carousel. Tap the card or swipe to move to the
next one; the dots below jump straight to a screen.

| Screen | What it shows |
|---|---|
| `co2` | CO2 and a tall history chart |
| `co2_env` | CO2, temperature, humidity and a shorter chart |
| `big` | CO2 as large as it goes, framed in the color of its level |

The chart is rebuilt from Home Assistant's history, so it survives a restart
of the monitor.

## Installation

### HACS

1. In HACS, open the menu (⋮) and choose **Custom repositories**.
2. Add `https://github.com/OWNER/co2-monitor-card` with the type **Dashboard**.
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
| `screens` | all three | Which screens to show, in order: `co2`, `co2_env`, `big`. |
| `rotate_seconds` | `10` | Seconds per screen. `0` stops the carousel. |
| `chart_bars` | `40` | Bars in the chart: 20, 25, 40, 50 or 100. |
| `chart_bar_seconds` | `20` | Seconds per bar: 20, 30, 60, 120 or 300. |

Set `chart_bars` and `chart_bar_seconds` to the values on the monitor's own
settings page, and the card draws the same chart as the device.

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
