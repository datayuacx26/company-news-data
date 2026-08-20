---
schema_version: "1.0.0"
document_id: "aa5e1bcfa5dd34a993339982fd5451de4652ef3a90444f5e8b7e5911b031f1f9"
company_key: "yc-enode"
company: "Enode"
source_id: "yc-enode-news-import-98d1bcb59046"
canonical_url: "https://enode.com/blog/product-updates/set-charge-rate-limit-control-ev-charging-in-kw"
published_at: "2026-04-07T00:00:00+00:00"
first_seen_at: "2026-07-21T18:15:11.081738+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:fcafc3f1b9d6a6c705ea3dcdc00e048f77546c46144732dd479bb5e8dd7aec8f"
---

# Set Charge Rate Limit: Control EV charging in kW

The most valuable EV charging optimizations don't just manage *when* charging occurs, they also control *how fast* an EV charges.


**Reducing peak power costs**


In markets like Sweden, Norway, and Belgium, grid fees already include a capacity tariff component based on a household's highest power peaks during the month. An EV charging at 11 kW while the home is already drawing 3 kW creates a 14 kW peak, one that can significantly increase the monthly bill. Capping the charge rate so total consumption stays below a defined threshold helps reduce these costs.


Without charge rate limit. With charge rate limit.


**Maximizing solar self-consumption**


As rooftop solar adoption grows, so does the opportunity to charge EVs using surplus production instead of exporting it to the grid at unfavorable rates. But solar surplus fluctuates throughout the day, and turning charging on or off isn't precise enough to capture it. Adjusting the charge rate continuously to match available surplus captures far more clean energy. Charge at 3 kW when there are 3 kW to spare, ramp to 7 kW when the clouds clear.


Without charging modulation. With charging modulation.


Both use cases need charge rates in kW. Until now, that meant working in Amperes and dealing with voltage and phase conversions that vary across every charger and household.


**Set Charge Rate Limit changes that.** Send a target charge rate in kW and Enode handles the conversion to the charger's native protocol. No calculations needed on your side.


## Built for real-world charging programs


- **Persistent limits:** the charge rate limit stays in place across charging sessions until you explicitly change it, making it useful for capacity tariff optimization where you need to cap power draw consistently
- **Respects user overrides:** if the end user adjusts the rate through their charger's own app, Enode won't override it and will report the change via webhooks


## Available now for all customers


Set Charge Rate Limit is in Beta and available for the following charger brands: ChargeAmps, Easee, Garo, go-e, Keba, and Zaptec. Full Sandbox support and a Dashboard Asset View are also included.


See the[Set Charge Rate Limit](https://developers.enode.com/api/reference#postSetChargerChargeRateLimit) and[Get Charger](https://developers.enode.com/api/reference#getCharger) endpoints to start integrating.


## Looking ahead


Support for additional charger brands is being added over time. Charge rate control will also extend to vehicles via[EV Charger Pairing](https://developers.enode.com/docs/ev-charger-pairing/introduction) . If your users have linked a vehicle and charger, you'll be able to control the charge rate via the Vehicle API.
