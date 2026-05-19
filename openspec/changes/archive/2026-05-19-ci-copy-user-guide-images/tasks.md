## 1. myips-website — build workflow

- [x] 1.1 Add "Copy user guide images" step to `.github/workflows/build.yml` after the user guide copy step, using grep to parse image refs and realpath to resolve paths

## 2. myips — trigger workflow

- [x] 2.1 Add `assets/**` to the trigger paths in `.github/workflows/notify-website.yml`
