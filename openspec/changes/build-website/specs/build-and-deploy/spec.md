## ADDED Requirements

### Requirement: GitHub Actions workflow builds and deploys the site
The myips-website repo SHALL contain a GitHub Actions workflow (`.github/workflows/build.yml`) that checks out both repos, copies docs, builds Jekyll, and deploys to GitHub Pages.

#### Scenario: Workflow completes successfully on push to main
- **WHEN** a commit is pushed to the main branch of myips-website
- **THEN** the workflow runs, builds Jekyll, and deploys the updated site to GitHub Pages

---

### Requirement: Private myips repo checked out using PAT
The workflow SHALL check out the private `myips` repo using a PAT stored as a repository secret (`MYIPS_READ_TOKEN`). The PAT SHALL have `contents: read` access to the myips repo only.

#### Scenario: myips repo is accessible during build
- **WHEN** the workflow runs
- **THEN** the myips repo is cloned successfully and docs files are accessible

---

### Requirement: Docs copied with front matter injection before Jekyll build
The workflow SHALL run a shell step that copies `user-guide.md` and (if present) `privacy-policy.md` from the cloned myips repo into the Jekyll source tree, prepending appropriate front matter to each.

#### Scenario: Copied files have front matter
- **WHEN** the copy step completes
- **THEN** each copied markdown file begins with a Jekyll front matter block

---

### Requirement: Manual trigger available
The workflow SHALL support `workflow_dispatch` so it can be triggered manually from the GitHub Actions UI without requiring a push.

#### Scenario: Manual trigger runs the full build and deploy
- **WHEN** a user triggers the workflow manually via the GitHub Actions UI
- **THEN** the full build and deploy process runs

---

### Requirement: Workflow triggers on push to main and repository_dispatch
The workflow SHALL trigger on: push to `main` branch of myips-website, `workflow_dispatch`, and `repository_dispatch` with event type `docs-updated`.

#### Scenario: repository_dispatch event triggers build
- **WHEN** a `repository_dispatch` event with type `docs-updated` is received
- **THEN** the workflow runs the full build and deploy
