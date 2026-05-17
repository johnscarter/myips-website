## Context

myips-website is a fresh Jekyll scaffold (minima theme, no real content) hosted on GitHub Pages at myips.health. The app it supports — MyIPS — is a React Native / Expo health summary app in a separate private GitHub repo. The website needs to publish three pages (home, user guide, privacy policy) and stay in sync with docs that live and are maintained in the private app repo.

## Goals / Non-Goals

**Goals:**
- Publish a working site at myips.health with home, user guide, and privacy policy pages
- Keep user guide and privacy policy content sourced from a single location (the myips repo)
- Automatically republish the site when those docs change in myips
- Visually match the MyIPS design system (colors, typography, spacing)

**Non-Goals:**
- Dynamic content, forms, or backend of any kind
- A blog or changelog section
- SEO optimisation beyond basic meta tags
- Screenshots in the user guide (placeholders remain until app is screenshot-ready)
- Converting the privacy policy .docx (done separately in the myips repo as a prerequisite)

## Decisions

### D1: GitHub Actions over native GitHub Pages Jekyll build

**Decision:** Use a custom GitHub Actions workflow instead of the default GitHub Pages Jekyll build.

**Rationale:** The default Pages build can only access the single repository it's building. Checking out the private `myips` repo requires a PAT, which only a custom workflow can use. The Actions approach also gives full control over the build pipeline (copy step, front matter injection).

**Alternative considered:** Git submodule with default Pages build. Ruled out because the myips repo is private — the Pages build bot cannot clone private submodules.

---

### D2: Copy docs at build time, not symlinks or submodules

**Decision:** The GitHub Actions workflow copies `user-guide.md` and `privacy-policy.md` from the checked-out myips repo into the Jekyll source tree before building.

**Rationale:** Symlinks don't travel with the repo and break on other machines. Submodules require the referenced repo to be accessible to whoever clones the website repo — not viable with a private source. A copy-at-build-time script is simple, portable, and requires no local setup beyond having the PAT configured.

**Front matter injection:** The source docs in myips have no Jekyll front matter (they're plain markdown). The workflow prepends the appropriate front matter (`layout`, `title`, `permalink`) during the copy step using a small shell script.

---

### D3: Auto-trigger via repository_dispatch

**Decision:** A workflow in the `myips` repo (`notify-website.yml`) sends a `repository_dispatch` event to `myips-website` whenever `docs/user-guide/user-guide.md` or `docs/privacy-policy.md` change on the main branch.

**Rationale:** This keeps the two repos decoupled — myips does not need to know the deployment details of myips-website, only that it should signal a rebuild. The website workflow listens for the event and handles the rest. A manual `workflow_dispatch` trigger is also included on the website workflow as a fallback.

**PAT requirements:**
- `myips` repo secret `WEBSITE_DISPATCH_TOKEN`: fine-grained PAT with `contents: read` and Actions `write` (repository_dispatch) on myips-website
- `myips-website` repo secret `MYIPS_READ_TOKEN`: fine-grained PAT with `contents: read` on myips

A single PAT scoped to both repos can serve both purposes if preferred.

---

### D4: Custom Jekyll theme over minima override

**Decision:** Replace minima entirely with a minimal custom layout (`_layouts/default.html`) and a single stylesheet (`assets/css/main.css`) built from the MyIPS design tokens.

**Rationale:** Minima's default styles (system fonts, generic palette) would need extensive overriding to match the MyIPS design system. Starting from a clean custom layout is less CSS to maintain than fighting the theme. The site has only three pages and no complex components — a lightweight custom layout is entirely sufficient.

**Design tokens extracted from `MyIPS-StyleGuide.html`:**
- Font: DM Sans (body), DM Mono (code) — loaded from Google Fonts
- Primary palette: `#1A7285` (brand), `#0D3D47` (dark), `#EDF7FA` (light surface)
- Neutral palette: `#1C2B33` (text), `#5A6C76` (secondary), `#D5DEE2` (border)
- Spacing: 8pt grid (`4px` → `64px`)
- Radius: `6px` (sm), `10px` (md), `16px` (lg)
- Shadows: defined in tokens, applied sparingly

The style guide HTML file remains the source of truth for design tokens; the website CSS is a one-time extraction. If the brand changes significantly, the CSS is updated manually.

## Risks / Trade-offs

**PAT expiry** → The auto-trigger and private-repo checkout both depend on PATs. If a PAT expires, the build silently fails (or the trigger stops firing). Mitigation: use fine-grained PATs with the longest available expiry, and set a calendar reminder to rotate. GitHub will also send an email warning before expiry.

**Privacy policy prerequisite** → The site can be built and deployed before the privacy policy exists, but the /privacy/ page will 404 until the .md file is present in myips. Mitigation: home page links to /privacy/ should only be added once the file exists; or a placeholder page can be published in the interim.

**Screenshot placeholders in user guide** → The guide renders blockquote-style screenshot callouts where images will eventually go. These look fine on the web but are slightly jarring. Mitigation: accepted for now; screenshots added to the guide in a future pass.

**Single-page user guide length** → The guide is one long markdown file. On the web this becomes a very long page. Mitigation: accepted per user preference (no refactoring of the guide structure at this time); a floating ToC or sticky nav could be added later.

## Open Questions

- Should a placeholder /privacy/ page be published until the .docx-to-markdown conversion is done, or should the page simply be omitted from the nav until ready?
- Should the home page feature list reference the specific IPS sections the app covers, or keep it higher-level?
