## ADDED Requirements

### Requirement: Custom Jekyll layout replaces minima
The site SHALL use a custom `_layouts/default.html` in place of the minima theme. The minima gem SHALL be removed from `_config.yml` and `Gemfile`.

#### Scenario: Site builds without minima
- **WHEN** Jekyll builds the site
- **THEN** no minima theme files are referenced and the build completes without errors

---

### Requirement: Design tokens from MyIPS style guide applied
The site stylesheet SHALL implement the MyIPS design tokens: DM Sans (body text), DM Mono (code/mono), the teal primary palette (`#1A7285` brand, `#0D3D47` dark, `#EDF7FA` light), neutral palette (`#1C2B33` text-primary, `#5A6C76` text-secondary, `#D5DEE2` border), 8pt spacing grid, and defined border radii.

#### Scenario: Correct font renders on page load
- **WHEN** a user loads any page
- **THEN** body text renders in DM Sans loaded from Google Fonts

#### Scenario: Brand colour applied to navigation and headings
- **WHEN** a user loads any page
- **THEN** the site header and primary headings use the teal primary palette

---

### Requirement: Responsive layout
The site layout SHALL be readable on mobile viewports (320px+) and desktop. Navigation SHALL collapse or stack appropriately on small screens.

#### Scenario: Mobile viewport renders without horizontal scroll
- **WHEN** a user views any page on a 375px-wide viewport
- **THEN** no horizontal scrollbar appears and all content is readable

---

### Requirement: Navigation links to all three pages
The site header SHALL include navigation links to Home, User Guide, and Privacy Policy. The active page link SHALL be visually distinguished.

#### Scenario: User guide link present in navigation
- **WHEN** a user loads any page
- **THEN** the navigation contains a link to /user-guide/

#### Scenario: Active page highlighted
- **WHEN** the user is on the User Guide page
- **THEN** the User Guide nav link is visually distinguished from the others
