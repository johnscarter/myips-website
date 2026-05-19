## Why

The CI build workflow copied the user guide markdown but not the images it references, causing broken images on the published site. The trigger workflow also didn't fire when images were added or updated in the source repo.

## What Changes

- Add a build step that parses `user-guide.md` for image references and copies only those images into the website's `assets/` directory
- Broaden the `notify-website` trigger in the `myips` repo to fire on changes to `assets/**`, not just the markdown docs

## Capabilities

### New Capabilities

- `ci-user-guide-image-sync`: Parses the user guide markdown at build time to extract referenced image paths, then copies exactly those images from the source repo into the site's assets — no glob patterns, no manual maintenance

### Modified Capabilities

- none

## Impact

- `.github/workflows/build.yml` in `myips-website` — new "Copy user guide images" step added after the user guide copy step
- `.github/workflows/notify-website.yml` in `myips` — `assets/**` added to trigger paths
