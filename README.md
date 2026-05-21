# nextredo.github.io 🔗➟🌐
## Development
> [!WARNING]
> To avoid leaking metadata via EXIF data, do the following. <br>
> Run `git config core.hooksPath hooks` <br>
> This sets your git to use the pre commit hook for EXIF removal <br>

```bash
# Build & serve site locally (incl. drafts)
hugo server -D

# Clean up cached items
hugo build --gc
```

## Licensing 🔏📄
Everything is covered by [the license file](./LICENSE), unless in a subdirectory with its own LICENSE file or equivalent.
