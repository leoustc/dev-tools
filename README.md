# autogit

A small helper to hide in-progress dev work in a hidden remote ref, then release as a single squashed commit onto `main`. Also includes an installer to drop the script into `/usr/local/bin/autogit`.

## Commands

- `./autogit push`  
  - Work on the local `dev` branch. Stages everything, creates the first dev-only commit (or amends it on subsequent pushes), and force-pushes to `origin refs/hidden/dev`.

- `./autogit pull`  
  - Recreate local `dev` from `origin refs/hidden/dev`. Refuses to run if the working tree is dirty to avoid deleting local work.

- `./autogit release`  
  - From a clean tree, squashes `dev` into `main`, prompts for the final release message, commits, pushes `main`, force-updates `refs/hidden/dev` to the new `main` tip, and deletes local `dev` to start the next cycle fresh.

- `./autogit install` (or `./autogit update`)  
  - Copies the script to `/usr/local/bin/autogit` and makes it executable (may need `sudo`).

## Workflow

1) `./autogit push` while developing to keep work only in the hidden ref.  
2) `./autogit release` to publish: resolves conflicts if prompted, enter the release message, pushes `main`, and reseeds the hidden ref.  
3) Next cycle: `./autogit pull` to recreate `dev` from the hidden ref seeded at the latest release.

## Notes

- Requires a remote named `origin` and branches `main` and `dev`.  
- Uses force-push to `refs/hidden/dev`; `release` uses a squash merge to preserve `main` history.  
- Refuses to operate with a dirty working tree for destructive actions (pull/release).  
- Installer writes to `/usr/local/bin`; use `sudo` if needed.
