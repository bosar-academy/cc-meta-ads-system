# Meta Ads + Claude Code — Full System

Master index for the 9-component Meta Ads + Claude Code bundle. Each component is its own GitHub repo so you can install pieces independently OR run the master install prompt below to install everything at once.

I walk through the full system in this video: https://youtu.be/20jn29dZyfA

## The 9 Components

| # | Component | Repo | What it does |
|---|---|---|---|
| 1 | Business Context Setup | [cc-business-context](https://github.com/bosar-academy/cc-business-context) | Interview prompt → CLAUDE.md + 8 context files (identity, audience, voice, etc.). The foundation everything else reads from. |
| 2 | Competitor Discovery | [cc-competitor-discovery](https://github.com/bosar-academy/cc-competitor-discovery) | Scans Meta Ad Library for competitors in any niche. Ranks by ad longevity + volume + geo spread. |
| 3 | Funnel Spy | [cc-funnel-spy](https://github.com/bosar-academy/cc-funnel-spy) | 7-section teardown of any competitor's funnel (architecture, offer ladder, tech stack, sales mechanics). |
| 4 | Ad Strategy | [cc-ad-strategy](https://github.com/bosar-academy/cc-ad-strategy) | Turns research + context into a full Meta campaign brief with YAML handoff. Ships with a starter playbook. |
| 5 | Nano Banana (Image Gen) | [cc-nano-banana](https://github.com/bosar-academy/cc-nano-banana) | Generates static ad creatives via Gemini Nano Banana Pro. See `META-ADS-INSTALL.md` in that repo for ad-specific setup. |
| 6 | Funnel & Tracking Setup | [cc-meta-tracking-setup](https://github.com/bosar-academy/cc-meta-tracking-setup) | Walks through pixel install, custom conversions, UTMs, System User token. |
| 7 | Meta Ads (skill + CLI) | [cc-meta-ads](https://github.com/bosar-academy/cc-meta-ads) | Operate Meta ads end-to-end via official Meta CLI + Marketing API SDK. PAUSED-first iron rule. |
| 8 | Analytics Dashboard | [meta-ads-dashboard](https://github.com/bosar-academy/meta-ads-dashboard) | Next.js web app. Live Meta data + AI daily briefings + creative leaderboard + decision triggers. Replaces Ads Manager. |
| 9 | Master Install | [cc-meta-ads-system](https://github.com/bosar-academy/cc-meta-ads-system) | THIS REPO. One paste, all 9 components installed. |

## Quick install (recommended)

See [INSTALL.md](./INSTALL.md). Copy-paste the master install prompt into Claude Code. It walks through the entire system in 60-90 minutes.

## Pick-and-mix install

Each component has its own `INSTALL.md` with a standalone Part 1 + Part 2 if you only want a piece. The component repos:

- [cc-business-context/INSTALL.md](https://github.com/bosar-academy/cc-business-context/blob/main/INSTALL.md)
- [cc-competitor-discovery/INSTALL.md](https://github.com/bosar-academy/cc-competitor-discovery/blob/main/INSTALL.md)
- [cc-funnel-spy/INSTALL.md](https://github.com/bosar-academy/cc-funnel-spy/blob/main/INSTALL.md)
- [cc-ad-strategy/INSTALL.md](https://github.com/bosar-academy/cc-ad-strategy/blob/main/INSTALL.md)
- [cc-nano-banana/META-ADS-INSTALL.md](https://github.com/bosar-academy/cc-nano-banana/blob/main/META-ADS-INSTALL.md)
- [cc-meta-tracking-setup/INSTALL.md](https://github.com/bosar-academy/cc-meta-tracking-setup/blob/main/INSTALL.md)
- [cc-meta-ads/INSTALL.md](https://github.com/bosar-academy/cc-meta-ads/blob/main/INSTALL.md)
- [meta-ads-dashboard/INSTALL.md](https://github.com/bosar-academy/meta-ads-dashboard/blob/main/INSTALL.md)

## How the pieces fit together

```
                  ┌──────────────────────────────┐
                  │  1. Business Context Setup   │  (CLAUDE.md + context/)
                  └─────────────┬────────────────┘
                                │ feeds business context to all skills below
                                ▼
        ┌───────────────────────┴───────────────────────┐
        │                                               │
        ▼                                               ▼
┌─────────────────────┐                       ┌──────────────────────┐
│ 2. Competitor       │                       │ 6. Funnel & Tracking │
│    Discovery        │                       │    Setup             │
└─────────┬───────────┘                       │  (pixel, token,      │
          │                                   │   custom conversion) │
          ▼                                   └──────────┬───────────┘
┌─────────────────────┐                                  │
│ 3. Funnel Spy       │                                  │
└─────────┬───────────┘                                  │
          │                                              │
          ▼                                              │
┌─────────────────────┐                                  │
│ 4. Ad Strategy      │                                  │
│  (with starter      │                                  │
│   playbook)         │                                  │
└─────────┬───────────┘                                  │
          │                                              │
          │  produces YAML campaign brief                │
          ▼                                              │
┌─────────────────────┐         ┌──────────────────┐    │
│ 5. Nano Banana      │ ──────▶ │ 7. Meta Ads      │ ◀──┘
│  (statics)          │         │  (CLI + SDK,     │
└─────────────────────┘         │   PAUSED-first)  │
                                └──────────┬───────┘
                                           │
                                           ▼
                                ┌──────────────────────┐
                                │ 8. Analytics         │
                                │    Dashboard         │
                                └──────────────────────┘
```

## Higgs Field (video generation)

NOT included in this bundle. Follow the workflow in the video tutorial (https://youtu.be/20jn29dZyfA at ~32 min) - sign up at higgsfield.ai, install their CLI, integrate with Claude Code. The pattern is the same as Nano Banana but for AI-generated UGC video ads.

## Total cost

| Item | Cost |
|---|---|
| All 9 components | $0 (open source) |
| Apify (competitor-discovery) | ~$0.50/run, free $5/mo tier |
| Anthropic API (dashboard) | ~$5-15/mo |
| Gemini API (Nano Banana) | Free tier covers 100+ images/day |
| Meta API | Free (you pay actual ad spend) |
| Turso DB (dashboard cloud deploy) | Free tier |
| Vercel (dashboard cloud deploy) | Free tier |
| **Total ongoing** | **~$5-15/mo** + your ad budget |

## Maintenance

This bundle reflects the state of Meta's API (v25) and Claude Code as of mid-2026. Meta updates its API and CLI regularly - if a command breaks, check the official docs at developers.facebook.com/docs/marketing-apis. If a quirk in the v25 quirks section of `cc-meta-ads/SKILL.md` no longer applies, the skill will still work but may be doing unnecessary work.

If you find a meaningful issue, open a GitHub issue on the relevant component repo.
