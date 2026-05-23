# zk-doctor-action: ZK Pipeline Doctor GitHub Action

[![Marketplace](https://img.shields.io/badge/GitHub%20Marketplace-zk--doctor--action-purple?logo=github&style=for-the-badge)](https://github.com/marketplace/actions/zk-pipeline-doctor)
[![Latest](https://img.shields.io/badge/Latest-v1.1.0-blue?style=for-the-badge)](https://github.com/Battam1111/zk-doctor-action/releases/latest)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

Audit your ZK project on every push and PR. Free. 5-minute install.

```yaml
- uses: actions/checkout@v4
- uses: Battam1111/zk-doctor-action@v1
```

That's it. Within 30 seconds of pushing, you get a 6-detector health report in your Actions tab.

## What gets detected

8 ZK ecosystem signatures + 6 health dimensions:

- **Languages**: Compact (Midnight), Leo (Aleo), Noir (Aztec), Cairo (Starknet + Cairo M)
- **Rust zkVMs**: risc0, SP1, Plonky3, Stwo, OpenVM, Nexus, Jolt
- **Solidity ZK verifiers** (pairing + verify-function heuristic)
- **Health**: language, tests, CI, docs, security, reproducibility

Powered by the open-source [zk-pipeline-doctor](https://github.com/Battam1111/zk-pipeline-doctor) CLI.

## Modes

### Minimal (just print the report)
```yaml
- uses: Battam1111/zk-doctor-action@v1
```

### Fail PRs that regress (v1.1.0+)
```yaml
- uses: Battam1111/zk-doctor-action@v1
  with:
    comment-mode: 'diff'         # only show delta from base branch
    fail-on-regression: 'true'   # fail CI if PR drops score >0.1
    output: 'zk-doctor-report.md'
    comment-on-pr: 'true'
permissions:
  pull-requests: write
  contents: read
```

When `comment-mode: 'diff'`, the action runs zk-doctor on both PR HEAD **and** the base branch (via a git worktree), then posts a compact PR comment showing the score delta with an emoji-coded verdict.

## Inputs

| Name | Default | Description |
|---|---|---|
| `path` | `.` | Directory to audit |
| `format` | `markdown` | `markdown` or `json` |
| `output` | _(none)_ | Write report to file |
| `threshold` | `0.0` | Fail action if overall score < this (0.0–10.0) |
| `comment-on-pr` | `false` | Post the report as a PR comment |
| `comment-mode` | `full` | `full` (entire report) · `diff` (delta vs base) · `none` |
| `fail-on-regression` | `false` | Fail CI if PR score is lower than base by >0.1 (`diff` mode only) |

## Outputs

| Name | Description |
|---|---|
| `report-path` | Path to the generated report file |
| `overall-score` | Numeric overall score (0.0–10.0) |
| `base-score` | Overall score on base branch (PR + `diff` mode only) |
| `score-delta` | head - base (signed, PR + `diff` mode only) |

## What's next

The action is **free, MIT, and complete**. Two things you can add on top:

1. **[$99 Pre-Flight Audit](https://polar.sh/checkout/polar_c_gXO0FivhPZEULEbuWnpznkLPFdL2Koz68AvG93YoWFb)**: we run the same engine on your repo + narrate findings + personally review before delivering. 24h turnaround. [See sample](https://battam1111.github.io/bounty-radar-data/audits/sample.html). Pre-flight before a $10-50k human audit, NOT a substitute.

2. **[Bounty Radar](https://polar.sh/checkout/polar_c_BbZbN6eJnZ7rwsUfT1pMsj4lTftwnfMoGdWBo0KozKU)** ($19-497/mo) (real-time ZK bounty alerts to Telegram / webhook / Slack). Filter by ecosystem, reward, keywords. [Compare tiers](https://battam1111.github.io/midnight-zk-cookbook/pricing.html#radar).

## License

MIT © 2026 Battam1111

<!-- related-projects:start -->
## Related projects

- [**zk-pipeline-doctor**](https://github.com/Battam1111/zk-pipeline-doctor); the underlying CLI
- [**bounty-radar-data**](https://battam1111.github.io/bounty-radar-data/); live ZK bounty feed
- [**bounty-radar-mcp**](https://github.com/Battam1111/bounty-radar-mcp); MCP server for the feed
- [**midnight-zk-cookbook**](https://battam1111.github.io/midnight-zk-cookbook/); 17 ZK tutorials
<!-- related-projects:end -->
