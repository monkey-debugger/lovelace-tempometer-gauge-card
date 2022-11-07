# Lovelace tempometer-gauge-card

A Home Assistant lovelace custom gauge card for barometer, thermometer, humidity meter or anything you want with custom icons.

![pressure-and-temp](https://user-images.githubusercontent.com/25659602/106396921-2dc16900-640b-11eb-9921-baabe2fdb378.png)
![humidity-and-custom](https://user-images.githubusercontent.com/25659602/106397020-a9231a80-640b-11eb-882e-3b38cde7fa69.png)

## Usage
Add this card via HACS (recommended)

Or manually :
Add this custom card to your home assistant instance. Reference it into your lovelace configuration.
```
  - type: js
    url: /local/lovelace/tempometer-gauge-card.js
```

Finally :
Add it as a custom card to your lovelace : `'custom:tempometer-gauge-card'`.

Note :
This card is made for a full row, don't try to put it in a grid or horizontal stack, it will look bad. I won't fix this.

## Options
### Card options
| **Option**                | **Type**                        | **Description**                                                                                                                                                           |
|---------------------------|---------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `entity` ***(required)*** | string                          | The entity to track. Can be followed by an attribute to track `entity.attribute)`                                                                                         |
| `min` ***(required)***    | number                          | The gauge's minimum value                                                                                                                                                 |
| `max` ***(required)***    | number                          | The gauge's maximum value                                                                                                                                                 |
| `entity_min`              | string                          | The entity that define the minimum reached. Can be followed by an attribute to track `entity.attribute)` (you have to create this entity, the card will not compute it !) |
| `entity_max`              | string                          | The entity that define the maximum reached. Can be followed by an attribute to track `entity.attribute)` (you have to create this entity, the card will not compute it !) |
| `title`                   | string                          | Card title to show.                                                                                                                                                       |
| `card_style`              | string                          | Set this to `thermometer`, `humidity` or `custom` to change icons. (Default will be barometer theme, custom will need icon1, icon2, icon3 !)                              |
| `measurement`             | string                          | Custom unit of measurement                                                                                                                                                |
| `icon1`                   | string                          | Icon on left side in custom style.                                                                                                                                        |
| `icon2`                   | string                          | Icon on center in custom style.                                                                                                                                           |
| `icon3`                   | string                          | Icon on right side in custom style.                                                                                                                                       |
| `icon_color`              | string                          | Icon Color (Default var(--paper-item-icon-color))                                                                                                                         |
| `severity`                | [severity list](#severity-list) | Severity list to change the gauge color.                                                                                                                                  |
| `decimals`                | number                          | Decimal precision of entity value.                                                                                                                                        |

#### Severity list
Will match to the first entry in the list that is in range.

| **Option**             | **Type** | **Description**                                                                                        |
|------------------------|----------|--------------------------------------------------------------------------------------------------------|
| color ***(required)*** | string   | Name of the color, one of red, green, yellow, normal for builtin colors, or any other valid CSS color. |
| min                    | number   | Minimal value (inclusive) to match against. Matches if not set.                                        |
| max                    | number   | Maximal value (inclusive) to match against. Matches if not set.                                        |

Example:
```yaml
severity:
  - color: red
    min: 0
    max: 10
  - color: green
    min: 10
    max: 100
```

## Tip
The maximum and minimum bounds have a hover tooltip with their own values.

## Full examples
```yaml
type: 'custom:tempometer-gauge-card'
entity: sensor.barometer
min: 980
max: 1050
entity_min: sensor.barometer_min_this_week
entity_max: sensor.barometer_max_this_week
title: Barometer
decimals: 0
severity:
  - color: red
    max: 950
  - color: yellow
    min: 950
    max: 975
  - color: green
    min: 975
    max: 1025
  - color: white
    min: 1025
    max: 1035
  - color: "#000"  # always quote hex, yaml will see it as a comment otherwise
    min: 1035
```
```yaml
type: 'custom:tempometer-gauge-card'
entity: sensor.temperature
min: 15
max: 30
entity_min: sensor.temperature_min_this_week
entity_max: sensor.temperature_max_this_week
title: Thermometer
card_style: thermometer
severity:
  - max: 22
    color: green  # will match if temp is lower than 22
  - max: 24
    color: yellow  # will match if temp is lower than 24 but higher than 22
  - color: red  # will always match
```
```yaml
type: 'custom:tempometer-gauge-card'
entity: sensor.power
min: 0
max: 4000
entity_min: sensor.power_min_this_week
entity_max: sensor.power_max_this_week
title: Power Meter
card_style: custom
icon1: mdi:flash-off
icon2: mdi:flash-outline
icon3: mdi:flash
severity:
  - color: green
    min: 1000
    max: 1500
  - color: yellow
    min: 1500
    max: 2000
  - color: red
    max: 4000
  - color: black
    max: 5000
```

Maybe more to come ! PR are welcome and I can have a look to features requests.
