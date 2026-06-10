# nextredo.github.io 🔗➟🌐
> [!TIP]
> Website *should be* live at <https://nextredo.github.io>

## Development
> [!WARNING]
> To avoid leaking sensitive info via image EXIF metadata,
> run the following immediately after clone this repo: \
> `git config core.hooksPath hooks` \
> This sets your git to use the pre commit hook for EXIF removal.
>
> As this will sanitise but commit the unsanitised images, a follow up `git commit --amend --no-edit`
> is necessary to ensure the EXIF data does not exist in commit history.

## Installation

```bash
# Clone this repo with:
git clone https://github.com/nextredo/nextredo.github.io.git --recurse-submodules
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
# ! *Then follow the extra config steps below* !
# !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
```

## Updates
```bash
# 0. `cd` into the root of this repo
# 1. Check if there are any updates
git fetch --all --recurse-submodules

# 2. Pull this repo and submodules
git pull --all --recurse-submodules

# 3. Update the submodules (first time)
git submodule update --init --recursive

# 4. Update submodules (to checkout latest commit in main / master)
git submodule update --remote
```


```bash
# Build & serve site locally (incl. drafts)
hugo server -D

# Clean up cached items
hugo build --gc
```

## Licensing 🔏📄
Everything is covered by [the license file](./LICENSE), unless in a subdirectory with its own LICENSE file or equivalent.
