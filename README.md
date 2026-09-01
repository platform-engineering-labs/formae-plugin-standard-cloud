# formae-plugin-standard-cloud

Orbital metapackage naming the set of [formae](https://github.com/platform-engineering-labs/formae) plugins available on **hosted** installations:

- [aws](https://github.com/platform-engineering-labs/formae-plugin-aws)
- [azure](https://github.com/platform-engineering-labs/formae-plugin-azure)
- [gcp](https://github.com/platform-engineering-labs/formae-plugin-gcp)
- [k8s](https://github.com/platform-engineering-labs/formae-plugin-kubernetes)
- [auth-basic](https://github.com/platform-engineering-labs/formae-plugin-auth-basic)

## Why this exists next to `standard`

Its contents are currently identical to [`standard`](https://github.com/platform-engineering-labs/formae-plugin-standard), and that is deliberate rather than an oversight.

Hosted installations cannot install plugins on demand: plugins ride the agent image. The set they run is therefore a product decision, and it should be able to change without changing what self-hosted users get when they run `formae plugin install standard`. Keeping the two bundles separate is what makes that possible. This one is the hosted set; the other is the open-source default.

**Do not delete this as duplication.** The duplication is the point.

## Nothing installs it yet

This package is published, and no image installs it.

`ANY` requirements re-resolve at install time. Installing this bundle on top of an image that already carries `standard` upgrades any member with a newer release in the channel, which makes the resulting image's contents a function of the date it was built rather than of its declared inputs. That is measurable: installing an overlapping metapackage over a base image upgraded `k8s` from 0.1.9 to 0.1.10 while leaving explicitly pinned plugins alone.

When the hosted set actually diverges from `standard`, the shape to adopt is `EQ`-pinned member requirements rather than `ANY`, ordered ahead of any explicit per-plugin pins. That resolves deterministically and makes this file the single declarative statement of what hosted runs. Until then, cloud-only plugins are added as explicit pins alongside the base image's bundle.

## What reads this

Nothing in the formae MCP. Tooling that needs to know which plugins an installation has asks the installation, because that is the only source that is true at the time it is asked. This repository states intent for the image build, not runtime availability.

## How it builds

Pushing a tag (`X.Y.Z` or `X.Y.Z-dev[.N]`) triggers `.github/workflows/release.yml`, which dispatches `formae-actions/plugin-build.yml` with `kind=metapackage`. The dispatched workflow signs the metapackage with the `platform.engineering` intermediate cert and publishes it to the `community` orbital repo.

This repo holds no signing material — only a fine-grained PAT (`HUB_DISPATCH_PAT`) scoped to `actions:write` on `formae-actions`, set as a repo secret.

## License

[FSL-1.1-ALv2](LICENSE)
