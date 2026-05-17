## Why

MyIPS needs a public website at myips.health to host the user guide and privacy policy — content that app store submissions require and that users need to reference. The site must exist before App Store / Google Play submission.

## What Changes

- New Jekyll site replaces the default scaffold at the root of myips-website
- Custom theme built from the MyIPS design system (DM Sans/DM Mono, teal palette, 8pt grid) replaces the minima default
- Three pages published: home (factual overview + feature list), user guide, privacy policy
- User guide and privacy policy sourced directly from `myips` repo (private) — not duplicated in this repo
- GitHub Actions workflow builds and deploys the site to GitHub Pages (myips.health)
- Auto-trigger workflow in `myips` repo fires whenever user guide or privacy policy docs change, republishing the site automatically

## Capabilities

### New Capabilities

- `site-theme`: Custom Jekyll theme implementing the MyIPS design system
- `home-page`: Home page with factual app overview and feature list
- `user-guide-page`: User guide page sourced from myips/docs/user-guide/user-guide.md
- `privacy-policy-page`: Privacy policy page sourced from myips/docs/privacy-policy.md
- `build-and-deploy`: GitHub Actions workflow that checks out both repos, builds Jekyll, and deploys to GitHub Pages
- `doc-sync-trigger`: Workflow in myips repo that fires repository_dispatch to myips-website when docs change

### Modified Capabilities

## Impact

- Replaces default Jekyll scaffold (index.markdown, about.markdown, _config.yml)
- Requires a GitHub PAT secret in myips-website with read access to the private myips repo
- Requires a GitHub PAT secret in myips repo with repository_dispatch write access to myips-website
- Privacy policy must be converted from .docx to .md in the myips repo before the page can be published (prerequisite, tracked separately)
- No backend changes — static site only
