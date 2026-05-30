# Part 1: What You're Getting

The complete Meta Ads + Claude Code system — all 9 components installed by one master prompt. Replicates the full end-to-end system from my video on your machine, branded for your business, with your own API keys.

I walk through the complete system in the Meta Ads + Claude Code video here: https://youtu.be/_TODO_VIDEO_URL_

This is the SINGLE PROMPT that installs everything. If you only want some pieces, see [README.md](./README.md) for individual component install prompts.

## What Gets Installed

After running the master prompt:

1. **Business context** (CLAUDE.md + context/* files) in your business workspace
2. **5 Claude Code skills** at `~/.claude/skills/`:
   - `/competitor-discovery` - find Meta ad competitors in any niche
   - `/funnel-spy` - deconstruct competitor funnels
   - `/ad-strategy` - produce real campaign briefs (with starter playbook)
   - `/nano-banana` - generate static ad creatives via Gemini
   - `/meta-ads` - create + manage Meta campaigns via official CLI + SDK
3. **Tracking infrastructure** (Meta pixel + custom conversion + System User token + UTMs)
4. **Analytics dashboard** (Next.js Web app, local at first - optionally Vercel deploy)

## What You Need (gather these before running)

- **Meta Business account + ad account** - https://business.facebook.com
- **Claude Code** - https://claude.ai/code
- **Apify account** (free) - https://console.apify.com/sign-up (for `/competitor-discovery`)
- **Anthropic API key** - https://console.anthropic.com (for the dashboard's AI briefing)
- **Gemini API key** (free) - https://aistudio.google.com/apikey (for `/nano-banana`)
- **Python 3.10+** + **uv** + **Node.js 20+** (already on Mac if you have Homebrew)
- **A landing page you can edit** (for pixel install)
- **Optional:** Turso (free) for dashboard cloud deploy, Vercel (free) for hosting, Airtable for sales-pipeline tracking
- **60-90 minutes** uninterrupted

## Total Cost

| Item | Cost |
|---|---|
| The bundle itself | $0 |
| Apify | Free tier covers 15-100 runs/mo |
| Anthropic API | $5-15/mo |
| Gemini API | Free tier covers 100+ images/day |
| Meta API | $0 (you only pay actual ad spend) |
| Turso + Vercel | Free tier |
| **Ongoing** | **~$5-15/mo** + your ad budget |

## Heads Up

- **Order matters.** The master prompt sequences installs in dependency order: business context first (so skills produce non-generic output), then skills, then tracking, then dashboard.
- **It WILL ask for API keys.** Have them ready in a text file before starting so the interview doesn't stall.
- **Everything launches PAUSED.** The `/meta-ads` skill never activates campaigns without explicit confirmation.
- **One business workspace per business.** If you run multiple businesses, run the business-context step separately for each in different folders.

---

# Part 2: Copy-Paste This Into Claude Code

```
I want you to install the complete Meta Ads + Claude Code system from https://github.com/bosar-academy/cc-meta-ads-system - all 9 components in one go. Walk me through the full setup. Plan for 60-90 minutes.

Step 0 - Pre-flight interview:

Before installing anything, ask me ALL of these at once so I can gather them and paste back:

REQUIRED:
1. Apify API token (sign up at https://console.apify.com/sign-up, copy from https://console.apify.com/account/integrations)
2. Anthropic API key (https://console.anthropic.com)
3. Gemini API key (free at https://aistudio.google.com/apikey)
4. Meta Business account access (I'll generate the System User token together later - just confirm I have an account at business.facebook.com)
5. The directory I want my business workspace in (default: ~/Projects/my-business)
6. The business name (used in branding throughout the install)

OPTIONAL:
7. Turso account for cloud DB (free at turso.tech) - skip if I want local-only dashboard
8. Vercel account for deploying the dashboard (free at vercel.com) - skip if local-only
9. Airtable account + base for sales pipeline tracking - skip if I don't track pipeline in Airtable
10. Any existing brand knowledge base file (PDF, doc, markdown) to speed up the business context interview

Wait for me to paste back all the above. If anything required is missing, walk me through getting it before proceeding.

Then proceed in this exact order. Confirm completion at each step before moving on.

Step 1 - Business context (~20-30 min):

Clone https://github.com/bosar-academy/cc-business-context to /tmp/cc-business-context.

Cd to my business workspace directory (create if doesn't exist). Run the business context interview prompt (read /tmp/cc-business-context/prompts/business-context-interview.md and walk me through the 8 sections):
  1. Identity → context/identity.md
  2. Audience / ICP → context/audience.md
  3. Voice → context/voice.md
  4. Positioning → context/positioning.md
  5. Offers → context/offers.md
  6. Results → context/results.md
  7. Funnel → context/funnel.md
  8. Frameworks → context/frameworks.md

Then generate CLAUDE.md at the workspace root from /tmp/cc-business-context/templates/CLAUDE.md.template.

Confirm I can read CLAUDE.md and the 8 context files. The other skills read from these.

Step 2 - Competitor Discovery skill (~3 min):

Run:
  git clone https://github.com/bosar-academy/cc-competitor-discovery ~/.claude/skills/competitor-discovery

Set up Apify token:
  mkdir -p ~/.config/apify
  echo 'MY_APIFY_TOKEN' > ~/.config/apify/token
  chmod 600 ~/.config/apify/token

Smoke test:
  python3 ~/.claude/skills/competitor-discovery/helpers/discover.py --seed weight-loss-demo --country US --max-per-keyword 30

Expect: ~$0.05 charge to Apify, output to ~/competitor-research/weight-loss-demo/<today>/. Confirm report.md exists.

Step 3 - Funnel Spy skill (~2 min):

Run:
  git clone https://github.com/bosar-academy/cc-funnel-spy ~/.claude/skills/funnel-spy

No setup needed - uses Claude's built-in WebFetch. We'll smoke-test this later with the strategy step.

Step 4 - Ad Strategy skill (~5 min):

Run:
  git clone https://github.com/bosar-academy/cc-ad-strategy ~/.claude/skills/ad-strategy
  mkdir -p ~/.claude/context
  cp ~/.claude/skills/ad-strategy/templates/starter-playbook.md ~/.claude/context/media-buying-playbook.md

Ask me: "Do you have real media-buying call transcripts, agency notes, or a private playbook to use instead of the generic starter? (paste path / paste text / skip)"

If I provide content, append to ~/.claude/context/media-buying-playbook.md as a new "## My Practitioner Notes" section. If I skip, use the generic starter.

Step 5 - Nano Banana skill (~10 min):

Run:
  git clone https://github.com/bosar-academy/cc-nano-banana.git /tmp/cc-nano-banana
  cd /tmp/cc-nano-banana && ./install.sh --all

This copies the skill to ~/.claude/skills/nano-banana/ AND creates a Python venv inside with google-genai + pillow.

Write Gemini API key:
  echo 'GEMINI_API_KEY=MY_GEMINI_KEY' > ~/.claude/skills/nano-banana/.env

Then walk me through the brand profile (use Meta Ads defaults: aspect ratio 4:5). Ask me to confirm:
- Brand name (auto-fill from Step 1's identity.md)
- Default platform: "meta-ads" (sets 4:5 ratio)
- Primary color (auto-fill from identity.md if mentioned)
- Tone keywords (auto-fill from voice.md)
- Avoid list (auto-fill from voice.md anti-examples)

Write to ~/.claude/skills/nano-banana/config.json.

Smoke test by generating one test creative (4:5, premium dark cinematic background, headline "Test"). Save to /tmp/nano-banana-test.png. Confirm the image was generated.

Step 6 - Meta Tracking Setup (~15 min):

Clone https://github.com/bosar-academy/cc-meta-tracking-setup to /tmp/cc-meta-tracking-setup.

Walk me through /tmp/cc-meta-tracking-setup/prompts/tracking-setup-interview.md:
  1. Meta Business account + ad account verified
  2. System User token generated → written to ~/.config/meta/credentials (raw token, no KEY= prefix)
  3. Pixel ID + base snippet installed on my landing page
  4. Standard Event configured (Lead / CompleteRegistration / Purchase based on my funnel)
  5. Custom Conversion created in Events Manager (if my funnel needs one)
  6. UTM scheme applied
  7. Verified with Meta Pixel Helper Chrome extension
  8. Wrote tracking-config.md to my business workspace

This step has the most external clicking (Business Suite, Events Manager, my LP editor). Don't rush me.

Step 7 - Meta Ads skill (~10 min):

Run:
  git clone https://github.com/bosar-academy/cc-meta-ads ~/.claude/skills/meta-ads

Install official Meta CLI:
  curl -sSL https://developers.facebook.com/tools/ads-cli/install.sh | bash
  meta auth login    # opens browser - sign in with my Meta Business account

Verify:
  meta auth status

Install SDK Python via uv:
  uv tool install meta-ads
  ls ~/.local/share/uv/tools/meta-ads/bin/python    # confirm it exists

Write my ad account defaults to ~/.claude/skills/meta-ads/.env. Use values from Step 6 (tracking-config.md):
  META_AD_ACCOUNT_ID=act_...
  META_DEFAULT_PAGE_ID=...
  META_PIXEL_ID=...
  META_DEFAULT_LANDING_URL=https://...

Smoke test:
  meta auth status
  meta ads page list
  ~/.local/share/uv/tools/meta-ads/bin/python ~/.claude/skills/meta-ads/helpers/targeting.py search "Entrepreneurship"

Should print at least 3 interest IDs. If yes, the skill is wired.

Step 8 - Analytics Dashboard (~15-30 min):

Ask me where to clone the dashboard (default: ~/Projects/meta-ads-dashboard):
  git clone https://github.com/bosar-academy/meta-ads-dashboard <path>
  cd <path>
  npm install

Set up .env.local from .env.example with the values gathered in Step 0 + Step 6:
  DASHBOARD_PASSWORD=<I'll generate one>
  DATABASE_URL=file:./dev.db (local) OR TURSO config (cloud)
  ANTHROPIC_API_KEY=<my Anthropic key>
  META_MARKETING_TOKEN=<read from ~/.config/meta/credentials>
  META_AD_ACCOUNT_ID=<from tracking-config.md>
  META_CUSTOM_CONVERSION_ID=<from tracking-config.md if I have one>
  META_CAMPAIGN_LAUNCH_DATE=<I'll fill once I launch>

Ask me about Airtable: enable or skip.

Ask me about campaign plan (public/plan.md):
  (a) Paste /ad-strategy output (best - we'll do this after launching a real campaign)
  (b) Fill template manually
  (c) Skip - generic placeholder

Ask me about targets (lib/targets.ts): keep defaults or customize my CPR bands / decision gates now.

DB setup:
  DATABASE_URL="file:./dev.db" npx prisma generate
  DATABASE_URL="file:./dev.db" npx prisma db push

Smoke test:
  npm run dev    # in background
  Wait 5 sec then curl -X POST http://localhost:3000/api/refresh
  Open http://localhost:3000 in my browser

If I see the dashboard with data, the local install works.

Ask me about Vercel deploy:
  (a) Yes - walk through (push to private GitHub repo + Vercel import + env vars)
  (b) Skip - I'll deploy later or run local only

Step 9 - Final cheat sheet:

Print the trigger phrases for every skill so I can use them right away:

CONTENT CREATION:
  /nano-banana <prompt>     - generate ad creative (4:5 default for Meta)

RESEARCH:
  /competitor-discovery <niche> <country>     - find competitors
  /funnel-spy <brand or URL>                  - deconstruct funnel

STRATEGY:
  /ad-strategy <campaign theme>               - produce campaign brief

EXECUTION:
  Just describe what you want in natural English, e.g.:
    "Create a $50/day cold campaign for X targeting Y, with these 5 creatives"
    "Pause campaign LEADS_BIZ-OWNERS-US_2026-05-25"
    "Pull insights for my last 7 days"
    "Scale ad set X by 25%"

DASHBOARD:
  Local: http://localhost:3000 (after `npm run dev` in dashboard folder)
  Cloud: https://<my-vercel-url> (if deployed)
  Login: my DASHBOARD_PASSWORD

Tell me the system is ready. Suggest the next move:
  "Try the workflow end-to-end:
   1. /competitor-discovery <your niche>
   2. /funnel-spy <top result>
   3. /ad-strategy 'test campaign for <my offer>'
   4. (When ready) describe what you want to launch and I'll use /meta-ads to scaffold it PAUSED for your review."

That's it - end-to-end system installed. 60-90 minutes from "I want this" to "campaigns scaffolded paused for review."
```
