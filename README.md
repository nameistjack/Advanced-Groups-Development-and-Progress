# Public development site

This directory contains the hand-written landing, progress, and wiki pages for
the **Advanced Groups Development and Progress** GitHub Pages site. The site is
exported from the private AdvancedGroups repository into the separate public
`nameistjack/Advanced-Groups-Development-and-Progress` repository.

The deployment workflow stages these files and copies Markdown from
`Documentation/` into `docs/`. It explicitly excludes
`Documentation/STEAM_WORKSHOP_LISTING.txt` and does not publish repository source,
configuration, game assets, or credentials.

The private repository requires a `PUBLIC_DOCS_TOKEN` Actions secret with
Contents read/write access limited to the public documentation repository. The
public repository must use **GitHub Actions** as the Pages source. After both
workflows succeed, the site URL will be:

```text
https://nameistjack.github.io/Advanced-Groups-Development-and-Progress/
```

Only the generated documentation is pushed to the public repository. The
AdvancedGroups source repository remains private.
