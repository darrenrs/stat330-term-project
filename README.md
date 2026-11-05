# SAT Statistics and Math Aptitude

STAT 330 project by Thomas Clark and Darren Skidmore.

Website: https://darrenrs.github.io/stat330-term-project/

## Source pages

- `index.qmd`: project homepage.
- `proposal.qmd`: original proposal.
- `case-study.qmd`: developing case study and analysis.
- `ai-use.qmd`: AI-use and independent-verification record.

`_quarto.yml` explicitly selects these four pages for rendering. The older
`portfolio/` directory is retained as reference material and is not rendered.

## Local preview

The CI build uses R 4.6.1 and Quarto 1.10.19. Install the R dependencies once:

```r
install.packages(c("knitr", "rmarkdown", "scales", "tidyverse", "usmap"))
```

From the repository root, run:

```bash
quarto preview
```

To build without starting a preview server, run `quarto render`.
Both commands create local output in `docs/`, which Git ignores. Commit source
files, data, and configuration; generated HTML and site libraries are not tracked.

## Continuous integration and deployment

`.github/workflows/pages.yml` installs R, the required R packages and their system
dependencies, and Quarto. It executes the analysis, renders the website, verifies
that the four output pages exist, and uploads the rendered `docs/` as an Actions
artifact. It does not commit generated files or use a publication branch.

- Pull requests targeting `main` build the website without deploying.
- Pushes to `main` build and deploy to GitHub Pages if the build succeeds.
- The workflow can also be run manually from the Actions tab on `main`.
- Main-branch builds and deployments run one at a time so an older build cannot
  overwrite a newer site. Superseded PR builds are cancelled. A failed build
  leaves the last successful site published.

When the analysis needs another R package, add it to the dependency step in
`pages.yml` and to the local installation list above. Package versions are resolved
by the dependency action and cached; they are not locked to an exact snapshot.

### One-time GitHub setup

After committing and pushing the workflow, open **Settings → Pages → Build and
deployment** and set **Source** to **GitHub Actions**. Then run the workflow from
the Actions tab if it has not already deployed successfully. The default
`github-pages` environment must allow deployments from `main`.

The workflow uses GitHub's built-in token with read-only source access and Pages
deployment permissions. No personal access token or repository write permission
is needed. To make PR validation mandatory, configure a rule for `main` requiring
the **Render website** status check and merge changes through pull requests.

### Existing clones

This migration removes generated output from Git tracking. Before pulling it,
preserve any source edits and discard or move aside local changes to previously
tracked generated files under `docs/` and `portfolio/docs/`. After pulling, local
renders are ignored and do not appear in commits or cause HTML merge conflicts.

### Submission requirement

The original checkpoint asks for rendered `docs/` in the repository. This CI
configuration instead retains the rendered site as a downloadable Actions artifact
and publishes it to Pages. Document this change for the instructor and confirm
that the artifact satisfies that submission requirement.
