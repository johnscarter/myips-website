## 1. Prerequisites (manual, outside this repo)

- [x] 1.1 Convert `myips/docs/MyIPS-PrivacyPolicy-v2.docx` to `myips/docs/privacy-policy.md` and commit to myips main branch
- [x] 1.2 Create a fine-grained GitHub PAT with `contents: read` on the myips repo — save as secret `MYIPS_READ_TOKEN` in myips-website repo settings
- [x] 1.3 Create a fine-grained GitHub PAT with Actions write (repository_dispatch) on myips-website repo — save as secret `WEBSITE_DISPATCH_TOKEN` in myips repo settings
- [x] 1.4 Confirm GitHub Pages is configured to deploy from GitHub Actions (not from a branch) in myips-website repo settings

## 2. Jekyll Project Setup

- [x] 2.1 Update `_config.yml`: set `title`, `description`, `url` (https://myips.health), remove `twitter_username`/`github_username`, remove minima theme reference
- [x] 2.2 Update `Gemfile`: remove `gem "minima"`, add `gem "jekyll"` with appropriate version, keep `jekyll-feed` if desired
- [x] 2.3 Delete default scaffold files: `about.markdown`, `index.markdown`
- [x] 2.4 Create `_layouts/default.html` — base HTML layout with `<head>`, site nav (Home, User Guide, Privacy Policy), `{{ content }}`, and footer
- [x] 2.5 Create `assets/css/main.css` — extract design tokens from MyIPS-StyleGuide.html (CSS custom properties, Google Fonts import, base styles, nav styles, responsive breakpoints)

## 3. Home Page

- [x] 3.1 Create `index.md` with Jekyll front matter (`layout: default`, `title: MyIPS`, `permalink: /`)
- [x] 3.2 Write factual overview paragraph (2–4 sentences: what it is, IPS 2.0.0 / FHIR R4 standard, local-only storage)
- [x] 3.3 Write feature bullet list (local storage, IPS conformance, PDF + JSON export, clinical sections: medications, allergies, conditions, immunisations, lab results, patient story)
- [x] 3.4 Add body links to User Guide (`/user-guide/`) and Privacy Policy (`/privacy/`)

## 4. GitHub Actions — Build & Deploy Workflow (myips-website)

- [x] 4.1 Create `.github/workflows/build.yml` with triggers: `push` to main, `workflow_dispatch`, `repository_dispatch` (type: `docs-updated`)
- [x] 4.2 Add checkout step for myips-website repo
- [x] 4.3 Add checkout step for private myips repo using `MYIPS_READ_TOKEN` secret, checked out to a temp path (e.g. `_myips-src`)
- [x] 4.4 Add shell step to copy and inject front matter into user guide: prepend `---\nlayout: default\ntitle: User Guide\npermalink: /user-guide/\n---\n` then append content of `_myips-src/docs/user-guide/user-guide.md` into `user-guide.md`
- [x] 4.5 Add shell step to conditionally copy and inject front matter into privacy policy (only if file exists in `_myips-src/docs/privacy-policy.md`)
- [x] 4.6 Add Ruby / Bundler setup step and `bundle exec jekyll build` step
- [x] 4.7 Add GitHub Pages deploy step (using `actions/upload-pages-artifact` and `actions/deploy-pages`)
- [x] 4.8 Configure workflow permissions: `pages: write`, `id-token: write`, `contents: read`

## 5. GitHub Actions — Doc Sync Trigger (myips repo)

- [x] 5.1 In the myips repo, create `.github/workflows/notify-website.yml` with `push` trigger on `main` branch, path filters: `docs/user-guide/user-guide.md` and `docs/privacy-policy.md`
- [x] 5.2 Add step to send `repository_dispatch` event (type: `docs-updated`) to `johnscarter/myips-website` using `WEBSITE_DISPATCH_TOKEN` secret via `curl` or `peter-evans/repository-dispatch` action

## 6. Verification

- [x] 6.1 Run `bundle exec jekyll serve` locally and verify all three pages render correctly
- [ ] 6.2 Push to main — confirm GitHub Actions build workflow completes successfully
- [ ] 6.3 Confirm site is live at https://myips.health with correct content
- [ ] 6.4 Make a test edit to `user-guide.md` in the myips repo, push to main, and confirm myips-website rebuilds and the update appears on myips.health
