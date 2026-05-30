# Master Install — Meta Ads + Claude Code System

This is the reference prompt that powers the master install. It's what gets pasted into Claude Code via [INSTALL.md](../INSTALL.md). See INSTALL.md for the published Part 1 + Part 2 copy.

## Architecture notes for maintainers

The master prompt is intentionally a single self-contained chunk so users can copy-paste from Gumroad without context switching. Each step is independently re-runnable - if a buyer's session crashes at Step 5, they paste Step 5 alone from the relevant component INSTALL.md and continue.

Step dependencies (must run in order):
1. Business context (no deps, but every other skill produces sharper output if this runs first)
2. Competitor Discovery (Apify token)
3. Funnel Spy (none)
4. Ad Strategy (reads Business Context + Competitor Research + Funnel Teardowns)
5. Nano Banana (Gemini key + may read brand profile from Business Context)
6. Meta Tracking Setup (Meta Business account)
7. Meta Ads skill (needs token from Step 6, CLI install, SDK Python)
8. Dashboard (needs token + ad account + Anthropic key)

If any step fails, the master prompt should NOT silently proceed - it should stop and tell the user what's missing.

## Per-step gotchas the prompt should handle

- **Step 1:** "Lean Life" demo brand stays as the label for context files showing how the structure looks. Don't replace it - it's intentional.
- **Step 2:** Apify free tier ($5/mo) covers ~15-100 runs. Tell user.
- **Step 5:** install.sh creates a venv inside ~/.claude/skills/nano-banana/. If venv creation fails, fall back to system Python with `pip install --user google-genai pillow`.
- **Step 6:** Token MUST be raw (no `KEY=` prefix). Verify with `head -1 ~/.config/meta/credentials | wc -c` - should be ~200-300 chars.
- **Step 7:** `meta auth login` opens browser. If headless / SSH session, paste the printed URL into browser manually, then re-check `meta auth status`.
- **Step 8:** `npm run dev` should be run as a background task. Buyer needs to know to keep the terminal open while testing.

## When to update this prompt

- Meta releases a new Marketing API version → update the v25 references in Step 7
- Meta updates the CLI install URL → update Step 7 install command
- New component added to the bundle → add a new step in the dependency order
- A component's API changes → update its step's smoke test
