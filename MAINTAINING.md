# Maintaining rampcheck-data

Notes for the maintainer. The catalog, the crawler workflows and the host files arrive with milestone M4 of the app's
plan; this file grows with them.

## Vendors

RampCheck reads only public pages and only within each vendor's terms and robots.txt. The host files in
`catalog/hosts/` record what a vendor's terms say and what was agreed; the crawler and the app both enforce them.

### Identifying ourselves

| Who | User-Agent | When |
|---|---|---|
| Crawler (GitHub Actions in this repo) | `RampCheck-Crawler/<engine-sha7> (+https://github.com/RenderErr/rampcheck-data)` | a few times a day, at most 1 request per second per host (10 s on forum.pmdg.com) |
| App, client fallback | `RampCheck/<version> (+https://github.com/RenderErr/rampcheck-data)` | only when a user's list has no usable entry for a product they own, the host allows fallback, at most once every 6 h per page |

Both send GET requests only, no cookies, no per-user validators, and stop at once on 403, 429 or 430.

### Opt-out promise

If a vendor asks us to stop or to use another source, change the host file the same day (`crawler: false` or
`client_fallback: false`); the next list then narrows every app. Reply to the vendor when it is done.

### Mail templates

Send from the owner's address; never from a bot. Fill in the parts in `<…>`, keep the facts as they are, and write
links as text (`github.com/RenderErr/rampcheck-data`) if the mail tool rewrites URLs.

**Courtesy note** (the crawler reads public pages the terms do not restrict):

```
Subject: RampCheck: we read the version on your public <pages / changelog threads / feed>

Hi <vendor> team,

A short courtesy note. I'm <name>. I maintain RampCheck, a free, open-source (GPL-3.0) Windows app for
MSFS 2020 and 2024 that tells users which of their installed add-ons have an update and then opens the
vendor's own updater or product page, which for you means <updater or page>. It never downloads,
installs or redistributes anything. Details: https://github.com/RenderErr/rampcheck-data

To know the current version of <products>, a small crawler (a scheduled GitHub Action) reads
<the public page / thread / feed> a few times a day: GET only, at most <one request per second / your
Crawl-delay>, never anything robots.txt disallows, User-Agent
"RampCheck-Crawler/<build> (+https://github.com/RenderErr/rampcheck-data)".
<If client fallback is on for this host:> If a user's copy of the list is out of date, their app may
also check the page of a product they own directly, at most once every 6 hours, as
"RampCheck/<version> (+https://github.com/RenderErr/rampcheck-data)".
It publishes facts only: product, version, date and page URL.

If you'd rather we didn't, or prefer a different source, just let me know and we'll change it within
a day.

Thanks!
<name>
```

**Permission request** (the terms prohibit crawling or are unclear; nothing is fetched until the vendor agrees):

```
Subject: RampCheck: permission to read product versions from <site>?

Hi <vendor> team,

I'm <name>. I maintain RampCheck, a free, open-source (GPL-3.0) Windows app for MSFS 2020 and 2024.
It tells users which of their installed add-ons have an update and then opens the vendor's own updater
or product page. It never downloads, installs or redistributes anything.

For most vendors, a small crawler (a scheduled GitHub Action) reads one public page per product a few
times a day and publishes facts only: product name, version number, date and page URL. Details:
https://github.com/RenderErr/rampcheck-data

Your <Terms of Service, section N / Acceptable Use Policy> <rule out crawling and scraping / mention
personal purposes>, so RampCheck does not touch <site>, and your products currently show as
"no public source".

Would you allow RampCheck to read the current version of your products, for example from <page>?
GET only, a few requests a day, User-Agent
"RampCheck-Crawler/<build> (+https://github.com/RenderErr/rampcheck-data)". If you have or could offer
a small version feed or endpoint, I'd happily use that instead.

If the answer is no, that's completely fine and we'll keep your site out.

Thanks!
<name>
```

**Installer CDN question** (fragmenter `modules.json`, the same request the vendor's installer makes):

```
Subject: RampCheck: reading modules.json from <cdn host>, OK?

Hi <vendor> team,

I'm <name>. I maintain RampCheck, a free, open-source (GPL-3.0) Windows app for MSFS 2020 and 2024.
It tells users which of their installed add-ons have an update and then opens the vendor's own
updater, which for you means <installer>. It never downloads, installs or redistributes anything.
Details: https://github.com/RenderErr/rampcheck-data

To tell users whether their <products> build matches the current track, a small crawler (a scheduled
GitHub Action) would read your public installer config and, for each track, the modules.json on your
CDN: the same small request your installer makes, a few times a day, User-Agent
"RampCheck-Crawler/<build> (+https://github.com/RenderErr/rampcheck-data)". The app itself never
fetches from your CDN. It compares the hashes with the installed install.json and only says
"matches" or "differs from the selected track"; it never claims one build is newer than another.

Is that OK with you? If you'd prefer another endpoint, I'm happy to use it.

Thanks!
<name>
```

**Broken robots.txt heads-up** (send before or with a permission request):

```
Hi <vendor> team,

A small heads-up: the robots.txt on <site> seems to be <missing a line break / …>. It contains
"<exact lines>". A strict robots.txt parser reads that as "<effect>" for every crawler, so search
engines and other well-behaved bots may be told to skip your whole site. Probably not intended?
```

### Sent and open (2026-10-21)

Ten drafts were prepared in the owner's mailbox on 2026-10-20 (corrected 2026-10-21): courtesy notes to PMDG,
UK2000 and Contrail; CDN questions to FlyByWire, Headwind, and HUES/Horizon; permission requests to iniBuilds,
Parallel 42 and SoFly; a robots.txt heads-up plus permission request to FSDreamTeam. PMDG and Contrail have no public
mail address (contact form). Record each answer in the vendor's host file.
