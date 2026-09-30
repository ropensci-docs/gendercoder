# Create a custom dictionary from fuzzy matches

`gender_create_dictionary` suggests dictionary entries for gender
responses that are not already matched exactly. The returned named
character vector is intended to be reviewed before it is combined with a
built-in dictionary and passed to
[`recode_gender()`](https://docs.ropensci.org/gendercoder/reference/recode_gender.md).

## Usage

``` r
gender_create_dictionary(
  gender,
  dictionary = gendercoder::manylevels_en,
  max_distance = 1
)
```

## Arguments

- gender:

  a character vector of gender responses for recoding

- dictionary:

  a character vector whose names are known gender responses and whose
  values are replacement values

- max_distance:

  maximum edit distance allowed for a suggested match

## Value

a named character vector of suggested replacement values

## Examples

``` r
suggested <- gender_create_dictionary(
  c("maile", "unknown"),
  dictionary = manylevels_en,
  max_distance = 1
)
suggested
#> maile 
#> "man" 
```
