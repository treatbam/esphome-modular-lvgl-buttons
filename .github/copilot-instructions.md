# Copilot Instructions — esphome-modular-lvgl-buttons

This is an **ESPHome YAML library** for building LVGL touchscreen UIs. Not Python, not JavaScript — pure ESPHome YAML with inline C++ lambdas.

Read [ARCHITECTURE.md](../ARCHITECTURE.md) for full details.

## Key Rules

- **Always use named colors, never hex values.** Use library colors from `common/color.yaml` (e.g., `ep_orange`, `misty_blue`, `very_dark_gray`, `gray50`-`gray900`) or ESPHome built-in CSS color names (e.g., `coral`, `dodgerblue`, `teal`, `tomato`). See `common/color.yaml` for the full list. If you need a new color, define it as a substitution (e.g., `my_teal: "0x1ABC9C"`) and reference as `$my_teal` — never inline hex.
- Every button component needs a **unique `uid`** — reusing one causes LVGL ID compile errors
- Buttons extend pages via `!extend ${page_id | default("main_page")}` — they don't create pages
- Widget IDs follow the pattern: `button_${uid}`, `icon_${uid}`, `label_${uid}`, `widget_sensor_${uid}`
- Variables use `${var | default(value)}` syntax for defaults
- Colors use substitution variables: `$button_on_color`, `$button_off_color`, `$icon_on_color`, `$icon_off_color`, `$label_on_color`, `$label_off_color`
- Icons are substitution vars from `common/mdi_glyph_substitutions.yaml`: `$mdi_lightbulb`, `$mdi_home`, etc.
- Fonts: `$icon_font` for icons, `$text_font` for labels
- Grid positioning: `grid_cell_row_pos`, `grid_cell_column_pos`, `grid_cell_row_span`, `grid_cell_column_span`

## File Structure for New Buttons

1. YAML comment header listing all `vars:`
2. `lvgl: pages:` section using `!extend` with container → button → labels
3. State tracking section (`binary_sensor` or `text_sensor` from `platform: homeassistant`)
4. Color updates in the state handler using `lvgl.widget.update`

## Testing

```bash
esphome config example_code/SDL-lvgl-display_modular_480px.yaml
```

Or run full validation: `bash testing/test_configs.sh`
