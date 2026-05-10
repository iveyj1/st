# Upstreams and port notes

This repo starts from current suckless `st` and ports Luke Smith's `st` changes on top.

## Remotes

Expected remotes:

```sh
git remote -v
```

- `suckless`: upstream suckless `st`.
- `luke`: upstream Luke Smith `st`.
- `github`: personal GitHub repo.

Push should stay disabled for `suckless` and `luke`.

```sh
git remote set-url --push suckless DISABLED
git remote set-url --push luke DISABLED
```

## Divergence point

The shared ancestor between current suckless and Luke's repo was:

```text
0008519 FAQ: document the color emojis crash issue which affected some systems is fixed
```

Luke's repo then added many custom patches. Suckless continued upstream development to `0.9.3` and beyond.

## Ported changes

Main changes ported from Luke's repo:

- Scrollback support.
- Mouse wheel scrollback.
- Keyboard scrollback bindings.
- Alpha transparency.
- Focus/unfocus alpha behavior.
- Runtime alpha adjustment keys.
- Xresources loading.
- Gruvbox-style default colors.
- Clipboard copy/paste shortcuts.
- URL open/copy helpers.
- External pipe support.
- Command output copy helper.
- `st-urlhandler` script.
- `st-copyout` script.
- Anysize patch.
- Font2 fallback support.
- Boxdraw rendering.
- Harfbuzz ligature support.
- OSC color reload behavior.
- Dirty border clear fix.
- Manpage updates.
- Build updates for new files and libraries.
- Added `Xdefaults`, `README.md`, `PKGBUILD`, and `FUNDING.yml` from Luke's repo.

Current suckless fixes were kept, including:

- Version `0.9.3` changes.
- OSC 110/111/112 color reset handling.
- Zero-sized terminal guard in `tsetdirt()`.
- Other upstream fixes since the Luke divergence.

## Config policy

Upstream suckless tracks `config.def.h` and generates `config.h` locally.

Luke's repo tracks `config.h` directly.

This repo keeps Luke-style defaults in `config.def.h`.

Recommended policy:

- Track `config.def.h`.
- Do not track personal local edits in `config.h`.
- Regenerate `config.h` with `make` when missing.

If `config.h` is already tracked in your repo, decide once and keep policy consistent.

## Build

After merges or conflict fixes, run:

```sh
make clean
rm -f config.h
make
```

This verifies `config.def.h` can generate a working `config.h`.

## Merge from suckless upstream

Use this when suckless releases new changes.

```sh
git switch master
git status
git fetch suckless
git switch -c merge-suckless-$(date +%Y%m%d)
git merge suckless/master
```

Resolve conflicts.

Likely conflict files:

- `st.c`
- `x.c`
- `st.h`
- `win.h`
- `config.def.h`
- `Makefile`
- `config.mk`
- `st.1`

Then build:

```sh
make clean
rm -f config.h
make
```

Commit merge:

```sh
git add .
git commit
```

Then merge branch back if desired:

```sh
git switch master
git merge merge-suckless-YYYYMMDD
```

## Merge from Luke upstream

Use this when Luke's repo adds new custom patches.

```sh
git switch master
git status
git fetch luke
git switch -c merge-luke-$(date +%Y%m%d)
git merge luke/master
```

Resolve conflicts carefully.

Luke may modify older `st` code. Prefer current suckless code when it contains security fixes or upstream correctness fixes. Prefer Luke code when it implements one of the custom features listed above.

Pay special attention to:

- `config.h` vs `config.def.h`.
- `x.c` drawing and font code.
- `st.c` scrollback and external pipe code.
- Harfbuzz files: `hb.c`, `hb.h`.
- Boxdraw files: `boxdraw.c`, `boxdraw_data.h`.
- Scripts: `st-urlhandler`, `st-copyout`.

After conflict fixes:

```sh
make clean
rm -f config.h
make
```

Commit merge:

```sh
git add .
git commit
```

## Safer alternative to direct merges

For either upstream, use a temporary branch first:

```sh
git switch -c test-upstream-merge
git fetch suckless
# or: git fetch luke
git merge suckless/master
# or: git merge luke/master
```

If result is bad:

```sh
git merge --abort
# or, if already committed:
git switch master
git branch -D test-upstream-merge
```

## Push to personal GitHub

Push normal branches only to `github`.

```sh
git push -u github master
```

If GitHub already has unrelated history, push a new branch first:

```sh
git push -u github master:suckless-luke-port
```

Only overwrite GitHub `master` if intended:

```sh
git push --force-with-lease github master
```
