# nextredo.github.io 🔗➟🌐
## Development
> [!WARNING]
> To avoid leaking sensitive info via image EXIF metadata,
> run the following immediately after clone this repo: \
> `git config core.hooksPath hooks` \
> This sets your git to use the pre commit hook for EXIF removal.
>
> As this will sanitise but commit the unsanitised images, a follow up `git commit --amend --no-edit`
> is necessary to ensure the EXIF data does not exist in commit history.

```bash
# Build & serve site locally (incl. drafts)
hugo server -D

# Clean up cached items
hugo build --gc
```

## Licensing 🔏📄
Everything is covered by [the license file](./LICENSE), unless in a subdirectory with its own LICENSE file or equivalent.
