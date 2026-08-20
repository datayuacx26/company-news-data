---
schema_version: "1.0.0"
document_id: "59042a4bebbe7154b7c696b1342d45e69a789b1c30a8ebbbfce243f11c4259f8"
company_key: "yc-amal-invest"
company: "Amal Invest"
source_id: "yc-amal-invest-news-import-cb58b37e0a38"
canonical_url: "https://amalinvest.com/changelog/2026-06"
published_at: "2026-06-30T00:00:00+00:00"
first_seen_at: "2026-07-21T06:14:08.584317+00:00"
fetched_at: "2026-07-28T21:22:15.524600+00:00"
content_hash: "sha256:10319c2fcc1044cd97c13351da7cfd178857ad08ed1939c64268c9cec0e6af25"
---

# One login, two-factor auth, and a better mobile app

# One login, two-factor auth, and a better mobile app


June 30, 2026


---


### New


#### One login for Alpaca and Trading 212


Amal-on-Alpaca and Amal-on-Trading-212 used to be separate accounts with separate logins. You now have one Amal account: sign in once, pick your broker (or use both), and switch between them inside the app. You can set a default so you land where you expect.


If you bought the Trading 212 prelaunch and also had a free app account, your account has been upgraded to the plan you paid for automatically.


#### Two-factor authentication


You can now enable TOTP two-factor auth from account settings. Works with any authenticator app, comes with downloadable backup codes, and supports trusted devices so you're not typing a code every morning. It's opt-in, so go turn it on.


#### Today's movement on the dashboard


The portfolio summary now shows today's movement, the thing you actually open the app to check. The main dashboard button also adapts to your state (uninvested cash, funds without money, fully funded) and points at the next sensible step.


### Improved


#### Mobile


A bottom navigation bar with safe-area support, a back button in the header, page transitions, and loading skeletons instead of blank white flashes. The home page streams in section by section. The Trading 212 dashboard got a full redesign, and the roadmap board is finally usable on mobile and tablet.


#### /funds rebuilt


The /funds page predated the redesign and it showed. It now has a per-fund allocation breakdown and a detailed table with holdings, dividends, and realized P&L per fund. The NaN% some of you saw there is gone too. It was a divide by zero.


#### Stock pages


Stock detail pages now put the compliance verdict, your existing exposure, and the chart on one screen, built around the question you're actually asking: should I buy this?


#### Filter rules, shown honestly


The fund filters tab now has an outcome summary: how many holdings your rules kept, how much value was retained, and what's still awaiting screening. Risky setups (a fund we don't track, unrated assets allowed) are called out plainly with what to do about them. Physically backed funds like gold ETFs get their own panel instead of equity-screening language that never applied to them, and exclusion counts now show the real numbers.


#### Sign-in


Resetting your password now signs you straight in. Google sign-in shows the account chooser instead of silently picking the wrong account. The sign-in page highlights the method you used last time.


#### Transactional emails


All 17 of them redesigned onto one clean template.


### Fixed


- After a trade, the dashboard could show stale positions and cash for up to an hour. Fills now land within seconds of execution
- Double-clicking invest or liquidate while the screen hadn't caught up could queue a duplicate order. A second click is now refused while the first order is in flight
- Buying an individual stock is noticeably faster. We cut the dead time between opening the buy modal and confirming the order
- Fund pages no longer crash when our holdings data provider has an outage. We fall back to cached holdings and keep going
- Charts for ETFs with thousands of holdings (VXUS) could hang forever. Indexed and fast now
- Rebalances wait for sell orders to settle before sending the buys, so slow fills no longer cause half-finished rebalances
- Stale "pending" badges on stocks from stuck order intents now clear themselves
- A new nightly check compares your broker account against our records and flags any drift before you'd notice it
