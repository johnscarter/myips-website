## ADDED Requirements

### Requirement: Privacy policy published at /privacy/
The site SHALL publish the privacy policy at `/privacy/` using content copied from `myips/docs/privacy-policy.md` during the build process.

#### Scenario: Privacy policy loads at correct URL
- **WHEN** a user navigates to myips.health/privacy/
- **THEN** the privacy policy page renders with a 200 response

---

### Requirement: Source file copied at build time
The GitHub Actions workflow SHALL copy `docs/privacy-policy.md` from the checked-out myips repo into the Jekyll source tree before running `jekyll build`.

#### Scenario: Build copies privacy policy source file
- **WHEN** the GitHub Actions build workflow runs
- **THEN** privacy-policy.md from the myips repo is present in the Jekyll working directory before the build step

---

### Requirement: Jekyll front matter injected during copy
The copy step SHALL prepend Jekyll front matter (`layout`, `title`, `permalink: /privacy/`) to the copied file.

#### Scenario: Front matter present in copied file
- **WHEN** the copy step completes
- **THEN** the copied privacy-policy.md begins with a valid Jekyll front matter block

---

### Requirement: Build succeeds without privacy policy present
If `myips/docs/privacy-policy.md` does not exist at build time (e.g. .docx conversion not yet done), the build SHALL still succeed and the /privacy/ page SHALL be omitted rather than causing a build failure.

#### Scenario: Missing privacy policy does not break build
- **WHEN** the GitHub Actions workflow runs and privacy-policy.md does not exist in the myips repo
- **THEN** Jekyll builds and deploys successfully without a /privacy/ page
