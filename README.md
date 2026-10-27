# Will My Fucking Washing Dry Today?

A single-page site that checks the forecast where you are and tells you, bluntly, whether your washing will dry on the line today, tomorrow or this week.

Live at https://adamjdogstar.github.io/willmyfingwashing/ once GitHub Pages is on (Settings → Pages → Deploy from a branch → `main`, `/ (root)`).

## How it works

- **Forecast:** [Open-Meteo](https://open-meteo.com/), free and no API key. In the UK and Ireland it asks for the Met Office model (`ukmo_seamless`) and fills any gaps from Open-Meteo's default forecast. Places are found with Open-Meteo's geocoding search or the browser's location.
- **Drying model:** each hour runs the Penman-Monteith equation, adapted for fabric on a line:

  `λE = [Δ·Rn + ρa·cp·(es − ea)/ra] / [Δ + γ·(1 + rs/ra)]`

  - `Rn`: net radiation on the garment. It takes about a fifth of the sunshine on flat ground (`shortwave_radiation`) and loses longwave heat to the sky through half its view. Sky emissivity comes from the Brutsaert formula, corrected for `cloud_cover`, so a clear night can make it negative and dew forms on the washing.
  - `es`, `ea`, `Δ`: Tetens saturation vapour pressure, actual vapour pressure from humidity, and the slope of the saturation curve.
  - `γ`: psychrometric constant from `surface_pressure`.
  - `ra`: aerodynamic resistance from a flat-plate heat transfer coefficient (5.7 + 3.8·u W/m²K). The 10 m wind is scaled to line height and reduced for a sheltered garden.
  - `rs`: fabric resistance per load type.
  - `λE` ÷ latent heat gives kg of water per m² per hour. Dividing by the water each load holds per m² gives % dried per hour. The last 25% dries more slowly (falling-rate period), and rain (0.2 mm or more in an hour) re-wets the load.
- **Simulation:** from your hang-out time it adds up the rate in 15-minute steps until the load hits 100%. The verdict compares that finish time with sunset and any rain:
  - **Hell yes:** dry before sunset
  - **Just:** dry within 4 hours after sunset
  - **Nope:** rain gets it first, or the air is too damp
  - **Too late:** you missed today's window
- **Load types:** light 0.25 kg/m² and rs 20 s/m, mixed 0.45 and 50, heavy 0.8 and 100. These are in `LOADS` in `index.html`.

The radiation share, garden shelter factor and water per m² are calibrated by feel: a 25°C, 40% humidity, sunny, breezy day dries a mixed load in about 2 hours, and a 10°C grey UK day doesn't manage it. Tweak `evaporation()` and `LOADS` in `index.html` if your washing disagrees.

Everything is in `index.html`: no build step and no dependencies apart from two Google Fonts.
