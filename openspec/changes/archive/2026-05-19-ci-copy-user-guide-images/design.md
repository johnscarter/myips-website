## Context

The website build pulls `user-guide.md` from the private `myips` repo and injects Jekyll front matter. Images referenced in the markdown live in `myips/assets/` using relative paths (e.g. `../../assets/foo.png`). The Jekyll site serves images from `/assets/`, so the build must copy the referenced files into `myips-website/assets/` before Jekyll runs. Previously no image copying was done, and the trigger workflow had no awareness of the `assets/` directory.

## Goals / Non-Goals

**Goals:**
- Copy exactly the images referenced in `user-guide.md` — no more, no less
- Trigger a website rebuild automatically when images in `myips/assets/` are added or changed
- Require no manual maintenance as the user guide evolves

**Non-Goals:**
- Syncing images referenced in any other doc (e.g. privacy policy)
- Validating that referenced images actually exist (a missing image will surface as a build error naturally)
- Copying non-image assets

## Decisions

**Parse the markdown rather than glob-matching filenames.**
A glob like `*-ios*.png` would miss images that don't follow the naming pattern and would include images that aren't referenced. Parsing the markdown with `grep` gives an exact list of what the guide needs. The regex `!\[.*?\]\(\K[^)]+` extracts the path from every `![alt](path)` expression; absolute URLs are filtered out with a second `grep -v`.

**Resolve paths with `realpath` relative to the markdown file's location.**
Image paths in the markdown are relative to the doc's location (`docs/user-guide/`), not the repo root. Using `realpath` cleanly handles any number of `..` hops without string manipulation.

**Copy into the existing `assets/` directory in `myips-website`.**
Jekyll already serves files from `assets/`. Placing images there requires no config changes and keeps images alongside the other static assets already committed to the website repo.

**Trigger on `assets/**` in `notify-website.yml`.**
The `myips` trigger workflow previously only watched the two doc files. Adding `assets/**` ensures a push of new or updated images fires the website rebuild without requiring a simultaneous doc edit.

## Risks / Trade-offs

- **Image filename collisions** — if two docs reference images with the same filename from different directories, one will overwrite the other. → Acceptable for now; the user guide is the only doc using images.
- **`realpath` on a missing file returns an error** — if the markdown references an image that doesn't exist in `assets/`, the step will fail loudly. → This is desirable; it surfaces broken references before deploy.
- **`assets/**` trigger is broad** — any file pushed to `myips/assets/` will trigger a rebuild, including non-image files. → Low cost; rebuilds are cheap and the trigger is correct by intent.
