# rampcheck-data

Version data for **RampCheck**, a free, open-source Windows app for Microsoft Flight Simulator 2020 and 2024 that
tells simmers which of their installed add-ons have an update. RampCheck never installs or changes anything; it shows
the status and opens the vendor's own updater or product page.

This repository holds the data side:

- **Catalog:** which products exist, which public page publishes their current version, and how to recognise the
  installed package. Small, reviewed YAML files.
- **Crawler:** a scheduled GitHub Action that reads those public pages a few times a day and publishes one small
  signed version list that the app downloads.

Status: early development. The app itself is not released yet.

## For vendors

The crawler identifies itself with the User-Agent `RampCheck/<version> (+https://github.com/RenderErr/rampcheck-data)`.
It only sends GET requests to public product, changelog or feed pages, about one page per product every six hours,
at most one request per second per host. It never logs in, follows robots.txt and your terms, and publishes facts only:
product ids, version strings, dates and the page URL. No descriptions, images or logos.

If you'd rather we didn't read your pages, or you can offer a feed or an endpoint you prefer, open an issue in this
repository. We stop reading a site within a day of being asked.

## Licence

The data in this repository is dedicated to the public domain under [CC0 1.0](LICENSE). The RampCheck app is licensed
under the GPL-3.0-or-later.

Not affiliated with any add-on vendor or with Microsoft. Product names and trademarks belong to their owners.
