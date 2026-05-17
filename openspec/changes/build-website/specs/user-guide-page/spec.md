## ADDED Requirements

### Requirement: User guide published at /user-guide/
The site SHALL publish the user guide at `/user-guide/` using content copied from `myips/docs/user-guide/user-guide.md` during the build process.

#### Scenario: User guide loads at correct URL
- **WHEN** a user navigates to myips.health/user-guide/
- **THEN** the user guide page renders with a 200 response

---

### Requirement: Source file copied at build time
The GitHub Actions workflow SHALL copy `docs/user-guide/user-guide.md` from the checked-out myips repo into the Jekyll source tree before running `jekyll build`. The source file in myips SHALL NOT be modified.

#### Scenario: Build copies source file
- **WHEN** the GitHub Actions build workflow runs
- **THEN** user-guide.md from the myips repo is present in the Jekyll working directory before the build step

---

### Requirement: Jekyll front matter injected during copy
The copy step SHALL prepend Jekyll front matter (`layout`, `title`, `permalink: /user-guide/`) to the copied file. The original file in myips SHALL remain without front matter.

#### Scenario: Front matter present in copied file
- **WHEN** the copy step completes
- **THEN** the copied user-guide.md begins with a valid Jekyll front matter block containing layout, title, and permalink fields

---

### Requirement: Markdown renders correctly
All markdown in the user guide (headings, lists, code blocks, blockquotes) SHALL render correctly as HTML.

#### Scenario: Screenshot callout blockquotes render
- **WHEN** a user views the user guide page
- **THEN** screenshot placeholder blockquotes render as styled blockquote elements, not as broken markup
