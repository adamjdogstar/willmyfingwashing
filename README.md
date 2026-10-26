# Will My Fucking Washing Dry Today?

A single-page site that checks the forecast where you are and tells you, bluntly, whether your washing will dry on the line today, tomorrow or this week.

Live at https://adamjdogstar.github.io/willmyfingwashing/ once GitHub Pages is on (Settings → Pages → Deploy from a branch → `main`, `/ (root)`).

## How it works

- **Forecast:** [Open-Meteo](https://open-meteo.com/), free and no API key. In the UK and Ireland it asks for the Met Office model (`ukmo_seamless`) and fills any gaps from Open-Meteo's default forecast. Places are found with Open-Meteo's geocoding search or the browser's location.
- **Drying model:** each hour gets a drying rate (% of a load per hour) from:
  - how thirsty the air is: vapour pressure deficit from temperature and humidity, boosted by wind
  - sunshine: shortwave radiation in W/m²
  - Cold, fog and dew slow it down. Rain (0.2 mm or more in an hour) re-wets the load.
- **Simulation:** from your hang-out time it adds up the rate in 15-minute steps until the load hits 100%. The verdict compares that finish time with sunset and any rain:
  - **Hell yes:** dry before sunset
  - **Just:** dry within 4 hours after sunset
  - **Nope:** rain gets it first, or the air is too damp
  - **Too late:** you missed today's window
- **Load types:** light ×1.4, mixed ×1, heavy ×0.6.

The numbers are calibrated by feel: a warm, sunny, breezy day dries a mixed load in about 3 hours, and a cool overcast day doesn't manage it. Tweak `dryingRate()` in `index.html` if your washing disagrees.

Everything is in `index.html`: no build step and no dependencies apart from two Google Fonts.
