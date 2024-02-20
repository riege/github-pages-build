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

![](img/pages-settings.png | height=305)

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

Summary for your repository
* enabled GitHub Pages in repositiory settings. This requires administration permissions.  
* create top level `docs` folder in repository.
* create a file `index.md` with the `docs` folder.
* create a file `pages.yml` within direcory `.github/workflows` in repository as above.
* Commit and push to the repository.

Check the repository workflow `Actions`. There should be an action publishing the GitHub Pages.

### Other scenarios
More complex scenarios than just supporting markdown from one `docs` folder is possible,
but not (yet) supported by github-pages-build.

### Example
This repository also acts as an example, see `docs/index.md` and result on https://riege.github.io/github-pages-build 
