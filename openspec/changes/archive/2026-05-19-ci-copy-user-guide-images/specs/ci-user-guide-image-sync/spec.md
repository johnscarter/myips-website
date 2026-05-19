## ADDED Requirements

### Requirement: Build copies only images referenced in the user guide
The CI build step SHALL parse `user-guide.md` to extract all locally-referenced image paths, resolve each path relative to the markdown file's location, and copy each resolved file into the website's `assets/` directory before Jekyll runs.

#### Scenario: Images referenced in user guide are copied
- **WHEN** the build runs and `user-guide.md` contains one or more `![alt](path)` image references with relative paths
- **THEN** each referenced image file SHALL be present in `assets/` before the Jekyll build step executes

#### Scenario: Absolute URLs are not copied
- **WHEN** `user-guide.md` contains an image reference with an `http://` or `https://` URL
- **THEN** the build step SHALL skip that reference and not attempt to copy any file

#### Scenario: Unreferenced images in the source repo are not copied
- **WHEN** the `myips` repo contains image files in `assets/` that are not referenced in `user-guide.md`
- **THEN** those files SHALL NOT be copied into the website's `assets/` directory

#### Scenario: Missing referenced image causes build failure
- **WHEN** `user-guide.md` references an image that does not exist in the source repo
- **THEN** the build step SHALL fail with a non-zero exit code

### Requirement: Website rebuilds when source images change
The `notify-website` workflow in the `myips` repo SHALL trigger a website rebuild when any file under `assets/` is added or modified on the main branch.

#### Scenario: New image triggers rebuild
- **WHEN** a new image file is pushed to `myips/assets/` on main
- **THEN** the `notify-website` workflow SHALL dispatch a `docs-updated` event to `myips-website`

#### Scenario: Updated image triggers rebuild
- **WHEN** an existing image file in `myips/assets/` is updated on main
- **THEN** the `notify-website` workflow SHALL dispatch a `docs-updated` event to `myips-website`
