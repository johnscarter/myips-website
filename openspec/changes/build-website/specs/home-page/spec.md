## ADDED Requirements

### Requirement: Home page published at root
The site SHALL publish a home page at `/` with layout `home` (or `default`).

#### Scenario: Home page loads at root URL
- **WHEN** a user navigates to myips.health/
- **THEN** the home page renders with a 200 response

---

### Requirement: Factual app overview paragraph
The home page SHALL include a concise factual paragraph (2–4 sentences) describing what MyIPS is, what standard it implements, and that all data is stored locally on device. It SHALL NOT use marketing language, superlatives, or calls to action.

#### Scenario: Overview paragraph present
- **WHEN** a user loads the home page
- **THEN** a paragraph describing MyIPS appears above the feature list

---

### Requirement: Feature list
The home page SHALL include a bulleted list of key app features. Features SHALL include at minimum: local-only storage (no account, no server), IPS 2.0.0 / FHIR R4 standard conformance, export as PDF and FHIR Bundle JSON, and the clinical sections covered (medications, allergies, conditions, immunisations, lab results, patient story).

#### Scenario: Feature list rendered as bullets
- **WHEN** a user loads the home page
- **THEN** a bulleted list of features is visible

---

### Requirement: Links to User Guide and Privacy Policy
The home page SHALL include clearly labelled links to the User Guide page and the Privacy Policy page.

#### Scenario: User guide link on home page
- **WHEN** a user loads the home page
- **THEN** a link to /user-guide/ is present in the page body or navigation
