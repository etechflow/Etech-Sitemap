# Changelog

All notable changes to `etechflow/module-sitemap` are documented here.

## [Unreleased]

### Security: portal-only licensing (removes forgeable key path)
- The HMAC signing secret shipped inside `LicenseValidator`, so anyone with the
  module could compute a valid key for their own domain and paste it into admin
  config — no code edit needed. Removed the shipped secret entirely.
- Deleted `SECRET_FRAGMENTS`/`BUNDLE_SECRET_FRAGMENTS`, `computeKey()`,
  `computeBundleKey()`, `checkKey()` and the client-settable `isLocallyIssuedKey()`
  grace (issued_key/issued_at/ip_blocked).
- Validation is now portal-only: only portal-issued `SP-` keys are honoured and
  the portal's answer is final. Offline grace derives solely from a cached
  genuine portal success, which a customer cannot fabricate.
- Removed the `production_environment` toggle bypass (`isProductionEnvironment()`
  now always returns true).
- Rewrote the unit suite around portal-only behaviour, incl. a hard test that a
  forged `SP-` key with attacker-controlled config and no portal is rejected.

## [1.0.0] - 2026-06-11

### Added
- Initial release.
- XML sitemap generation for products, categories, CMS pages and custom URLs.
- Per-type, per-store-view `changefreq` and `priority`.
- Product `<image:image>` entries (gated by the module toggle and Magento's native product-image-include setting).
- `hreflang` alternates across the store views of a website.
- Exclude rules by product SKU, category ID and CMS identifier.
- Sitemap-index splitting at the 50,000-URL protocol limit.
- Multi-store / multi-website output; default store view writes the canonical `sitemap.xml`.
- On-demand generation via admin **Generate Now** and `bin/magento etechflow:sitemap:generate`.
- Nightly cron generation (opt-in).
