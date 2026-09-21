# formae-plugin-standard-cloud

Orbital metapackage naming the set of [formae](https://github.com/platform-engineering-labs/formae) plugins available on **hosted** installations:

- [aws](https://github.com/platform-engineering-labs/formae-plugin-aws)
- [azure](https://github.com/platform-engineering-labs/formae-plugin-azure)
- [gcp](https://github.com/platform-engineering-labs/formae-plugin-gcp)
- [kubernetes](https://github.com/platform-engineering-labs/formae-plugin-kubernetes)
- [auth-basic](https://github.com/platform-engineering-labs/formae-plugin-auth-basic)

## Why this exists next to `standard`

This bundle includes AWS, Azure, GCP, Kubernetes and auth-basic, and requires
formae 0.90.2 or later. Kubernetes OIDC identity is configured by the hosted
installation and requires explicit access grants in each cluster; installing
this bundle does not create or grant that access. The self-hosted
[`standard`](https://github.com/platform-engineering-labs/formae-plugin-standard)
bundle is managed independently.

Hosted installations cannot install plugins on demand: plugins ride the agent image. The set they run is therefore a product decision, and it should be able to change without changing what self-hosted users get when they run `formae plugin install standard`. Keeping the two bundles separate is what makes that possible. This one is the hosted set; the other is the open-source default.

## Installing the hosted bundle

Install `standard-cloud` to select the hosted plugin set. Member dependencies
are pinned to exact versions so a given bundle release has a reproducible set.
Explicit plugin overrides can be applied after the bundle when needed.

Installing a metapackage does not uninstall packages inherited from another
bundle. An image switching from `standard` must also remove that metapackage
and any excluded plugins; updating this bundle alone does not change existing
installations.

## What reads this

Nothing in the formae MCP. Tooling that needs to know which plugins an installation has asks the installation, because that is the only source that is true at the time it is asked. This repository states intent for the image build, not runtime availability.

## How it builds

Pushing a tag (`X.Y.Z` or `X.Y.Z-dev[.N]`) triggers `.github/workflows/release.yml`, which dispatches `formae-actions/plugin-build.yml` with `kind=metapackage`. The dispatched workflow signs the metapackage with the `platform.engineering` intermediate cert and publishes it to the `community` orbital repo.

This repo holds no signing material — only a fine-grained PAT (`HUB_DISPATCH_PAT`) scoped to `actions:write` on `formae-actions`, set as a repo secret.

## License

[FSL-1.1-ALv2](LICENSE)
