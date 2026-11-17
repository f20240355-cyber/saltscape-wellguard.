# WellGuard — SaltScape Desert Water Early Warning

**Tagline:** Know before the well goes silent.

A zero-backend, GitHub Pages-ready interactive demonstration for the desert-water early-warning hackathon. **All well observations are deterministic, realistically shaped *synthetic* data**; not measurements of actual UAE wells. This is an educational proof of concept, **not a production life-safety system**.

## What it does

- Monitors four illustrative desert water sources with 70 daily synthetic observations each. Users can upload their own CSV (`day,level,tds`) for one selected well; this data is not verified.
- Predicts **water-level threshold** and **TDS investigation-trigger** crossings for 14/21/28/42 days.
- Shows a **10th–90th percentile scenario forecast band** and simulated threshold-crossing frequency for both hazards. Probability estimates are **uncalibrated model scenario frequencies**, not certified confidence.
- Dynamically creates a basic-phone-format SMS with lead time, alert class, uncertainty caveat and next steps.
- Simulates offline store-and-forward queuing when mobile coverage disappears; **no real SMS is sent**.
- Injects sensor outages and intermittent missing observations to demonstrate robustness and data-quality caveats.
- Changes warning sensitivity (30%/50%/70%) to illustrate false-alarm versus missed-warning tension.
- Exports observations and forecasts as CSV and provides a small synthetic walk-forward forecast-error check.

## Optional CSV import

Click **Import well readings CSV** under Forecast lab. Columns: `day,level,tds`, where `day` is a numerical relative-day index, `level` is metres relative to a local reference, and `tds` is mg/L. Provide at least 10 chronological rows including 8 complete measurements. The latest row is reindexed to day 0. Readings are kept only in the current browser session. **Thresholds are illustrative** and should be configured and reviewed by local experts before any operational use.

## Quick deploy

Upload `index.html`, `style.css`, `app.js`, and this `README.md` to the **root** of a new public GitHub repository. In **Settings → Pages** choose **Deploy from a branch**, `main`, `/(root)`. No install, API key, billing or backend needed. Open `index.html` in a browser for local testing.

## Demo script (90 seconds)

1. Open Southern Camp Well. Describe the two ways usable supply can fail: falling level and rising salinity.
2. Show the 28-day chart, percentile band and estimated threshold-crossing rate. Mention **all data is synthetic**.
3. In **Desert resilience**, simulate sensor outage. Observe data confidence and revised alert/verification language.
4. Enable network outage; queue the warning; simulate restoration. Show the basic-phone SMS preview and copy it.
5. Change sensitivity and explain why earlier warnings trade off false alarms versus missed crises.

## Methods

Every synthetic source has a seeded trend, cyclic variation and noise for water level in metres and a TDS proxy in mg/L. Each time-series is fitted using a recent-observation weighted linear regression. A 320-member deterministic scenario ensemble samples observed residuals, trend variation and future noise. The 10th and 90th percentiles of each day's simulated scenarios form a projection band. The proportion of scenarios that cross the operational level trigger or TDS trigger over the selected horizon is the model's **scenario crossing frequency**. These scenario frequencies are not empirically calibrated failure probabilities.

Missing observations are excluded from the fit; missingness, the age of the most recent reading, and observation span influence a visible **data confidence-quality indicator**. If evidence is insufficient, the system should prioritize verification instead of generating an overconfident quantitative warning. A lightweight walk-forward test reports level MAE on synthetic data only, not a real forecasting benchmark.

The example TDS trigger of 1,050 mg/L is an **arbitrary local investigation trigger** for the synthetic scenario and **not** a drinking-water or livestock safety limit. WHO notes that TDS impacts palatability and has no WHO health-based guideline value; **TDS alone cannot establish microbiological or chemical safety**. Any real deployment would require local expert calibration, appropriate water testing, and operational accountability.

## Requirements coverage

| Requirement | Demo implementation |
|---|---|
| Source measurements | Four seeded synthetic well time-series; clearly labelled |
| Future prediction | 14–42 day level and TDS forecasts |
| Confidence | 80% empirical scenario band, crossing frequency and data-quality label |
| Actable lead-time warning | Generated SMS-length message with specific follow-up action |
| Missing sensors and cloudy imagery | Missing data injectors; quality flags and verification language |
| Basic phones / patchy coverage | Text-only SMS preview, copy/download, simulated relay queue |
| False alarms / misses | Adjustable crossing-frequency warning threshold |
| End-to-end demo | Data → model → bands → classification → warning → relay |

## Limitations and deployment path

- The app does **not** ingest live sensor or satellite feeds. Sensor/imagery absence is demonstrated by dropping synthetic observations; satellite imagery isn't actually processed.
- The example regional names and locations are illustrative, not statements about real monitored sites.
- A store-and-forward SMS interface is simulated in the browser only; a real SMS provider or radio gateway would be needed to reach basic phones.
- Confidence bands are exploratory, not validated prediction intervals. No calibrated false alarm/miss-rate analysis on actual records is claimed.
- The dual-hazard combined crossing frequency uses a simplifying independence approximation, which may not hold in reality.
- Real deployment would need telemetry ingestion, weather/CHIRPS context, retraining, independent holdout evaluation, local threshold-setting, security and delivery acknowledgements.

## Research context

- WHO TDS guidance: https://www.who.int/docs/default-source/wash-documents/wash-chemicals/tds.pdf
- USGS Water Data: https://waterdata.usgs.gov/
- NASA GRACE: https://grace.jpl.nasa.gov/
- CHIRPS rainfall: https://www.chc.ucsb.edu/data/chirps

## Team

Add team members and actual roles here before submission.

Built during the hackathon; do not represent this prototype as deployed infrastructure.
