# r-wercker-base

Docker image for testing R packages on [wercker](http://wercker.com/), based on rocker/drd.
Includes R in the current release, and R-devel (accessible via `RD` and `RDscript`).

The `Dockerfile` contains machinery to install system and R dependencies via a simple configuration file.  The R dependencies are installed for both R and R-devel.


## Using this image

Create a file `wercker.yml` with the following contents:

```
box: rwercker/base
build:
  steps:
    - script:
        name: install
        code: if ! Rscript -e "devtools::install_deps()"; then fail "Install failed"; fi
    - script:
        name: check
        code: if ! Rscript -e "devtools::check()"; then fail "Check failed"; fi
```


## Adapting this image

Fork this repository, and edit the `PACKAGES` file.  It is a DCF file with the following sections, each section contains packages to be installed in one entry per line:

- `apt`: System packages to be installed using `apt-get install`
- `cran_packages`: R packages to be installed via `install.packages`
- `gh_packages`: R packages to be installed via `devtools::install_github`
- `deps`: R packages to be installed with all "Suggests" via `devtools::install_github(dependencies = TRUE)`

Edit the `Dockerfile` to change maintainer information and perhaps base image -- you may want to base your image on this image to save build time.  Add as "automated build" to [Dockerhub](https://hub.docker.com/), add a "repository link" to the base image (`rocker/drd` or your custom base image) to stay up to date, and that's it!
