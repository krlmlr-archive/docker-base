# r-wercker-base

Docker image for testing R packages on [wercker](http://wercker.com/), based on rocker/drd.


## Using this image

Create a file `wercker.yml` with the following contents:

```
box: krlmlr/r-wercker-base
build:
  steps:
    - script:
        name: install
        code: if ! Rscript -e "devtools::install_deps()"; then fail "Install failed"; fi
    - script:
        name: check
        code: if ! Rscript -e "devtools::check()"; then fail "Check failed"; fi
```
