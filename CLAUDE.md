# Doctor Yaad Links

Static linktree-style page at https://links.doctoryaad.com. Single point of outbound traffic distribution. Used in Instagram bio and YouTube channel About.

**Sister repo: [`Yaad182/doctoryaad-website`](https://github.com/Yaad182/doctoryaad-website)** → the main site at https://doctoryaad.com. Read its CLAUDE.md for the bigger picture (waitlist, analytics, Resend).

## Stack

Vanilla HTML + inline CSS + a small inline script. Two images (`avatar.jpg`, `vivo.jpg`). No framework, no build step, no dependencies. Hosted on Vercel.

## Deployment

GitHub auto-deploy connected. **`git push` to `main` deploys to production.**

## UTM propagation (the one non-obvious thing)

The outgoing doctoryaad.com links have hardcoded UTM params:

```
?utm_source=instagram&utm_medium=bio_link&utm_content=<button>
```

This is the **default** because Instagram bio doesn't pass any UTM params to this page, and IG is the primary inbound channel.

For other inbound sources (e.g. YouTube channel About sends visitors here with `?utm_source=youtube&utm_medium=channel_about`), the inline `<script>` at the bottom of `index.html` rewrites the outgoing links' `utm_source` and `utm_medium` to match the incoming params. `utm_content` per-button stays untouched so we keep button-level attribution.

Result in GA:

- IG bio → page → App card click: `instagram / bio_link / app_card`
- YouTube About → page → App card click: `youtube / channel_about / app_card`

**When adding a new doctoryaad.com link:** hardcode the Instagram fallback UTM and the script will adapt it for other sources automatically. Don't bother handling each source manually.

Sponsor/external links (movement-made.com, vivobarefoot.com, social profiles) are untagged — they're not our analytics.

## What this page doesn't do

- No GA tracking on this page itself. We don't see clicks here; we only see the destination on doctoryaad.com via UTM params. If button-level click data on the links page becomes important, add a cross-domain GA stream or a tiny event ping — currently not worth the complexity.
- No build pipeline. Edit `index.html` directly.

## Conventions

- No em-dashes or en-dashes (Yaad reads them as an AI tell).
- Brand colors match the main site: dark backgrounds, gold accent `#c9a84c`.

## External dashboards

- Vercel project: https://vercel.com/yaadmo-2540s-projects/doctor-yaad-links
- GA (lives on the main site, but tracks where this page sends traffic): https://analytics.google.com/analytics/web/#/p537610667/reports/intelligenthome
