# Building the docs

The Markdown files in this directory are built into a static documentation site with [Zensical](https://zensical.org/), using the official [`zensical/zensical`](https://hub.docker.com/r/zensical/zensical) Docker image. You only need Docker; there is nothing to install with Python or pip.

This file explains how to preview and build the site locally. It is excluded from the site itself.

## Files

| Path | Purpose |
| --- | --- |
| `zensical.toml` | Site configuration: name, navigation, logo, and the files that are left out of the build. |
| `stylesheets/extra.css` | Swaps in `assets/logo-white.svg` when the site is in dark mode. |
| `assets/` | The Forus logos. |
| `site/` | Build output. Generated, ignored by Git. |
| `.cache/` | Zensical build cache. Generated, ignored by Git. |

## Preview while you write

Run this from the `docs/` directory:

```bash
docker run --rm -it -p 8000:8000 -v "${PWD}":/docs zensical/zensical:0.0.74
```

Open <http://localhost:8000>. The site rebuilds and the browser reloads whenever you save a file. Press `Ctrl+C` to stop the server.

If port 8000 is already in use, map a different host port, for example `-p 8001:8000`, and open <http://localhost:8001>.

## Build the static site

Run this from the `docs/` directory:

```bash
docker run --rm -it -v "${PWD}":/docs zensical/zensical:0.0.74 build
```

The result is written to `docs/site/`. Open `docs/site/index.html` through a web server (for example the preview command above) rather than directly from disk, because the pages use directory-style URLs.

The build prints a warning for every broken link or missing anchor. Fix those before you open a pull request. Add `--strict` after `build` to make the build fail on warnings instead.

## Linux: file ownership

The image runs as `root`, so on Linux `site/` and `.cache/` end up owned by `root`. To create them as your own user, add `--user "$(id -u):$(id -g)"` to either command:

```bash
docker run --rm -it --user "$(id -u):$(id -g)" -v "${PWD}":/docs zensical/zensical:0.0.74 build
```

Docker Desktop on macOS and Windows maps ownership for you, so this is not needed there.

## Adding a page

1. Add the Markdown file under `docs/`.
2. Add it to the `nav` list in `zensical.toml`. Pages that are not listed there are still built, but they do not show up in the navigation.
3. Use relative links between pages, such as `[Local setup](local-setup.md)`. They work both on GitHub and in the built site.
4. Link to files outside `docs/` (for example `CONTRIBUTING.md` or files in the `backend` submodule) with full GitHub URLs. Relative links to those files break in the built site.

## Upgrading Zensical

The commands above pin the image to a specific version. To upgrade, check the available tags on [Docker Hub](https://hub.docker.com/r/zensical/zensical/tags), update the version in the commands in this file, and run a build to check for new warnings.
