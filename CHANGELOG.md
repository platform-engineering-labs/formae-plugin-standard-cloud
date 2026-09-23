# Changelog

All notable changes to the formae standard-cloud bundle are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

Install with `sudo formae plugin install standard-cloud` on the host that runs
the formae agent.

## [0.1.2]

Requires formae >= 0.90.2.

### Changed

- The hosted bundle now selects an exact, reproducible plugin set: AWS 0.1.18,
  Azure 0.1.15, GCP 0.1.18, Kubernetes 0.1.13, and auth-basic 0.1.0.
