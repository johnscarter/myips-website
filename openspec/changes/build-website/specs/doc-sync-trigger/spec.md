## ADDED Requirements

### Requirement: Trigger workflow in myips repo fires on doc changes
The myips repo SHALL contain a GitHub Actions workflow (`.github/workflows/notify-website.yml`) that fires when `docs/user-guide/user-guide.md` or `docs/privacy-policy.md` change on the `main` branch.

#### Scenario: Push changing user guide triggers notify workflow
- **WHEN** a commit changing `docs/user-guide/user-guide.md` is pushed to main in the myips repo
- **THEN** the notify-website workflow runs

#### Scenario: Push changing privacy policy triggers notify workflow
- **WHEN** a commit changing `docs/privacy-policy.md` is pushed to main in the myips repo
- **THEN** the notify-website workflow runs

#### Scenario: Push not touching docs does not trigger notify workflow
- **WHEN** a commit is pushed to main in myips that does not touch either doc file
- **THEN** the notify-website workflow does NOT run

---

### Requirement: Notify workflow sends repository_dispatch to myips-website
The notify workflow SHALL send a `repository_dispatch` event with type `docs-updated` to the `myips-website` repo using a PAT stored as a secret (`WEBSITE_DISPATCH_TOKEN`).

#### Scenario: repository_dispatch event received by myips-website
- **WHEN** the notify workflow runs
- **THEN** myips-website receives a `repository_dispatch` event with type `docs-updated` and its build workflow is triggered

---

### Requirement: Dispatch PAT scoped to myips-website only
The `WEBSITE_DISPATCH_TOKEN` PAT SHALL have the minimum required permissions: `contents: read` and Actions write (repository_dispatch) on myips-website only. It SHALL NOT have write access to the myips repo.

#### Scenario: Token has no write access to myips
- **WHEN** the WEBSITE_DISPATCH_TOKEN is configured
- **THEN** it cannot push commits or modify contents of the myips repo
