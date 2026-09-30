# recode_gender

`recode_gender` matches uncleaned gender responses to cleaned list using
an built-in or custom dictionary.

## Usage

``` r
recode_gender(
  gender,
  dictionary = gendercoder::manylevels_en,
  retain_unmatched = FALSE
)
```

## Arguments

- gender:

  a character vector of gender responses for recoding

- dictionary:

  a list that the contains gender responses and their replacement
  values. A built-in dictionary `manylevels_en` is used by default if an
  alternative dictionary is not supplied.

- retain_unmatched:

  logical indicating if gender responses that are not found in
  dictionary should be filled with the uncleaned values during recoding

## Value

a character vector of recoded genders

## Examples

``` r


df <- data.frame(
  stringsAsFactors = FALSE,
  gender = c("male", "MALE", "mle", "I am male", "femail", "female", "enby"),
  age = c(34L, 37L, 77L, 52L, 68L, 67L, 83L)
)

df$recoded_gender <- recode_gender(df$gender,
  dictionary = manylevels_en,
  retain_unmatched = TRUE
)
#> Results not matched from the dictionary have been filled with the user inputted values
df
#>      gender age recoded_gender
#> 1      male  34            man
#> 2      MALE  37            man
#> 3       mle  77            man
#> 4 I am male  52      I am male
#> 5    femail  68          woman
#> 6    female  67          woman
#> 7      enby  83     non-binary
```
