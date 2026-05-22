# ZK Pipeline Doctor — GitHub Action

[![Marketplace](https://img.shields.io/badge/GitHub%20Marketplace-zk--doctor--action-purple?logo=github)](https://github.com/marketplace/actions/zk-pipeline-doctor)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Powered by zk-pipeline-doctor](https://img.shields.io/badge/CLI-zk--pipeline--doctor-blue)](https://github.com/Battam1111/zk-pipeline-doctor)

Run a 6-detector health audit on every push or pull request to your zero-knowledge project. Fails CI if the score drops below your threshold.

**Detectors:** language (Compact / Leo / Noir / Cairo / Risc0) · tests · CI · docs · security · reproducibility.

---

## Quick start

### Minimal (just print the report)

```yaml
name: ZK audit
on: [push, pull_request]
jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Battam1111/zk-doctor-action@v1
```

### Fail PRs below 0.7 + comment the report

```yaml
name: ZK audit
on: [pull_request]
permissions:
  pull-requests: write
  contents: read
jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: Battam1111/zk-doctor-action@v1
        with:
          threshold: '0.7'
          output: 'zk-doctor-report.md'
          comment-on-pr: 'true'
```

---

## Inputs

| Name              | Default     | Description                                                                       |
|-------------------|-------------|-----------------------------------------------------------------------------------|
| `path`            | `.`         | Directory to audit                                                                |
| `format`          | `markdown`  | Output format: `markdown` or `json`                                               |
| `output`          | _(none)_    | Write report to file. Required for `comment-on-pr`.                               |
| `threshold`       | `0.0`       | Fail action if overall score < this. `0.0` disables gating.                       |
| `comment-on-pr`   | `false`     | Post the report as a PR comment. Requires `pull-requests: write` permission.      |

## Outputs

| Name           | Description                                                |
|----------------|------------------------------------------------------------|
| `report-path`  | Path to the report file (only when `output` is provided)   |

---

## What this action checks

The underlying CLI is open source: **[Battam1111/zk-pipeline-doctor](https://github.com/Battam1111/zk-pipeline-doctor)** (MIT). It runs six independent detectors that work for any ZK project — Compact, Leo, Noir, Cairo, Risc0, or generic — and emits a weighted overall score plus a prioritized fix list with concrete commands.

- **Language**: detects the dominant ZK language and validates toolchain config (`Cargo.toml`, `nargo.toml`, `leo.toml`, etc.)
- **Tests**: presence + ratio + framework conventions
- **CI**: workflow files, matrix coverage, key signals
- **Docs**: README sections, contribution guide, examples
- **Security**: dependency pinning, sensitive-file scan, secret patterns
- **Reproducibility**: lockfiles, fixed toolchain versions, deterministic build

Every finding has a concrete command to fix it. No vague suggestions.

---

## License

MIT. No paid tier at this time. See [zk-pipeline-doctor](https://github.com/Battam1111/zk-pipeline-doctor) for the underlying CLI.


---

## License

MIT © 2026 Battam1111

---

<!-- related-projects:start -->

## Related projects

- [**zk-pipeline-doctor**](https://github.com/Battam1111/zk-pipeline-doctor) — OSS CLI this action wraps
- [**zk-doctor-bot**](https://github.com/Battam1111/zk-doctor-bot) — GitHub App: deeper, model-narrated PR reviews
- [**midnight-zk-cookbook**](https://github.com/Battam1111/midnight-zk-cookbook) — currently in rollback; see DISCLOSURE there

<!-- related-projects:end -->
