# What is that?

This repo produces and publishes Windows binary builds of ungoogled-chromium with support for Certificate Authority of Russian Ministry of Digital Development and Communications .

Instead of blindly trusting any certificate issued by this CA, we trust them only for certain Russian web-sites (social networks, banks, government agencies and so on).

If any other website will present a TLS certificate issued by this CA, it will not be trusted.

Otherwise, this is fully tracking [upstream](//github.com/ungoogled-software/ungoogled-chromium-windows) packaging of ungoogled-chromium for Windows. No other changes.

# Why?

Certificate Authority of Russian Ministry of Digital Development and Communications is not trusted by overwhelming majority of software out there.

Even so, many Russian websites including many banks already use certificates issued by this CA, leaving users with following options:

1) Install state-issued CA system-wide and blindly trust it
2) Install and use Yandex Browser or Atom - local forks of Chromium produced by Yandex and Mail.ru respectfully
3) Ignore invalid certificate warnings and/or manually manage exceptions

This repo provides fourth option: to use a regular Chromium (or rather, ungoogled-chromium) which trusts to client certificates issued by Russian CA, but only for a limited subset of websites specified in [this list](scripts/russian_trusted_domains.txt).

By doing so, we lower the risk of MITM attack and avoid any additional tracking that may be present in other solutions.

In turn, the amount of changes this forks applies is minimal and release flow is out in the open, so everyone can take a five and make sure themselves that these builds are not malicious.

# How?

[embed_russian_ca.py](scripts/embed_russian_ca.py) modifies `chrome/browser/net/profile_network_context_service.cc` in build-time. The root then is added as an additional trust anchor. 

It is restricted by DNS name constraints to an allow-list of domains specified in [russian_trusted_domains.txt](scripts/russian_trusted_domains.txt). It does not become a general trust root, and it cannot vouch for arbitrary sites.

All of this is basically an adaptation of changes introduced by [Ruthenium](//github.com/rutheniumteam/ruthenium-android) to the desktop Chromium build, with one small distinction: they trust Russian-CA-issued client certs for every second-level domain in `.ru`, `.su` and `.рф` (`.xn--p1ai`). I believe that this is a bit too much, hence pre-defined domain list and this repo.

# Downloads?

Builds are available as [Releases](//github.com/giantplaceholder/ungoogled-chromium-windows/releases). Each release is built for three architectures (`x86`, `x86_64` and `arm64`) and contains a portable version (zipped) and a regular Chromium executable installer.

Download files built for your architecture, and either unzip or run the installer.

# Build on your host

Follow instructions from upstream, they work the same: 

https://github.com/ungoogled-software/ungoogled-chromium-windows

Original README is [kept](README.ORIGINAL.md) in this repo as well.

## License

Same as original, see [LICENSE](LICENSE)
