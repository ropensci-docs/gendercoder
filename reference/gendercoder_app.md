# Launch the gendercoder Shiny app

Code data interactively in a Shiny app that runs locally in RStudio or a
web browser using a bs4Dash interface. The app supports CSV, Stata,
SPSS, RDS, and R data files. Stata and SPSS files require the optional
haven package.

## Usage

``` r
gendercoder_app(...)
```

## Arguments

- ...:

  arguments to pass to
  [`shiny::runApp()`](https://rdrr.io/pkg/shiny/man/runApp.html)

## Value

Called for its side effect of launching a Shiny app.

## Examples

``` r
if (interactive()) {
gendercoder_app()
}
```
