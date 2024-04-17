# github-pages-build

Workflows and layouts to build GitHub pages from a Riege Software GitHub repository

## About

This repo provides a reusable workflow for GitHub Actions to be used to
build and deploy [GitHub Pages](https://docs.github.com/en/pages) content from a
GitHub repository.
This repo also provides a general layout with disclaimer, license, imprint etc.

## Usage

GitHub Pages offer a wide range of options in general.
Within Riege Software, our main use of GitHub Pages
is for public technical detail documentation for our clients, typically for
interfaces and/or APIs from/to our products.

The basic scenario is a private Riege Software repository which provides some
public information via GitHub Pages.

### Repositiory settings

The repository in question needs to be enabled for GitHub Pages.

Go to repository `Settings`, then on left side in section `Code and automation`
select `Pages`. Set
* GitHub Pages visibility to **Public** (not required if repository is already public)
* Build and deployment to **GitHub Actions**

**Screenshot:**

![](https://github.com/riege/github-pages-build/blob/main/img/pages-settings.png?raw=true|height=305)

### Basic simple scenario with one `docs` folder

The most simple scenario is a repository which contains a top level `docs` folder
containing a markdown file `index.md` plus optionally more content.

To use the reusable workflow, create a file `.github/workflows/pages.yml` in
the repository with the following content:

```yaml
name: Deploy static content to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]
    paths:
      - 'docs/**'
      - '.github/workflows/pages.yml'

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  pages:
    uses: "riege/github-pages-build/.github/workflows/pages-generate-from-docs-dir.yml@main"
```

Summary for your repository:
* Enable GitHub Pages in repositiory settings. This requires administration permissions.
* Create top level `docs` folder in repository.
* Create a file `index.md` with the `docs` folder.
* Create a file `pages.yml` within direcory `.github/workflows` in repository as above.
* Commit and push to the repository.
* Consider adding the topic `ghpages` to the `About` section on the main GitHub page of your
repository. This would add an easy method to search for our organisational repositories
which use this reusable workflow with the same layout via the link
https://github.com/search?q=org%3Ariege%20topic%3Aghpages&type=repositories

Check the repository workflow `Actions`. There should be an action publishing the GitHub Pages.

### More complex scenarios

- More complex scenarios than just supporting markdown from one `docs` folder is possible,
but not (yet) supported by github-pages-build.
- In theory every other complex scenarios can be covered by building an  `pages.yml` based
upon `pages-generate-from-docs-dir.yml` with individial enhancements.
- Another typical case is to copy some resources from underneath the `src` repository folder
into a subfolder of `docs/` and referring it from `.md` files.
Especially for this scenario it is possible to provide a from-folder-path and
a to-folder-path.
Currently, 2 sets are supported (`from1` and `to1`, `from2` and `to2`). This repository
itself demonstrates how it works by the following usage. Another common combination example is
`from1: src/main/resources/schemas` and `to1: schemas` and the `.md` file using links to
`schemas/some-filename.xsd`.

### Example

This repository also acts as an example, see `docs/index.md` and result on https://riege.github.io/github-pages-build

This repository uses `from1` and `to1` for demonstration purpose:
```
jobs:
  pages:
    uses: "riege/github-pages-build/.github/workflows/pages-generate-from-docs-dir.yml@main"
    with:
      from1: img
      to1: demo-img
```

