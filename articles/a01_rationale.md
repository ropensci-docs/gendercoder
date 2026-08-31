# Introduction to gendercoder

The goal of gendercoder is to allow simple recoding of free-text gender
responses.

### Installation

You can install the development version from GitHub with:

``` r

devtools::install_github("ropensci/gendercoder")
library(gendercoder)
```

### Why would we do this?

Researchers who collect self-reported demographic data from respondents
occasionally collect gender using a free-text response option. This has
the advantage of respecting the gender diversity of respondents without
prompting users and potentially including misleading responses. However,
this presents a challenge to researchers in that some inconsistencies in
typography and spelling create a larger set of responses than would be
required to fully capture the demographic characteristics of the sample.

For example, male participants may provide free-text responses as
“male”, “man”, “mail”, “mael”. Non-binary participants may provide
responses as “nonbinary”, “enby”, “non-binary”, “non binary”

Manually coding of such free-text responses this is often not feasible
with larger datasets.
[`recode_gender()`](https://docs.ropensci.org/gendercoder/reference/recode_gender.md)
uses dictionaries of common misspellings to re-code free-text responses
into a consistent set of responses. The small number of responses not
automatically re-coded by recode_gender() can then be feasibly manually
recoded.

### Motivating example

`gendercoder` includes a sample dataset with actual free-text responses
to the question “What is your gender?” from a number of studies of
English-speaking participants. The sample dataset includes responses
from 7756 participants. Naive coding identifies 103 unique responses to
this item.

``` r

library(gendercoder)

count_values <- function(x, name = "value") {
  counts <- sort(table(x, useNA = "ifany"), decreasing = TRUE)
  out <- data.frame(value = names(counts), count = as.integer(counts))
  names(out)[1] <- name
  out
}

knitr::kable(
  count_values(sample$Gender, "Gender"),
  caption = "Summary of gender categories before coding"
)
```

| Gender                                                            | count |
|:------------------------------------------------------------------|------:|
| female                                                            |  1836 |
| Male                                                              |  1755 |
| Female                                                            |  1722 |
| male                                                              |  1705 |
| FEMALE                                                            |   113 |
| Female                                                            |   113 |
| MALE                                                              |   107 |
| F                                                                 |    88 |
| f                                                                 |    58 |
| M                                                                 |    35 |
| m                                                                 |    33 |
| female                                                            |    24 |
| Male                                                              |    21 |
| woman                                                             |    19 |
| male                                                              |     7 |
| masculino                                                         |     7 |
| Man                                                               |     6 |
| man                                                               |     5 |
| Woman                                                             |     4 |
| female                                                            |     3 |
| Femail                                                            |     3 |
| femal                                                             |     3 |
| femail                                                            |     2 |
| Femalw                                                            |     2 |
| Frmale                                                            |     2 |
| Gender                                                            |     2 |
| Mail                                                              |     2 |
| MALE                                                              |     2 |
| Masculino                                                         |     2 |
| Nonbinary                                                         |     2 |
| male                                                              |     1 |
| Male                                                              |     1 |
| %                                                                 |     1 |
| 40                                                                |     1 |
| 54                                                                |     1 |
| Agender                                                           |     1 |
| agender (woman)                                                   |     1 |
| Androgynous                                                       |     1 |
| Apache Helicopter… Just kidding. There are only two. I am a Male. |     1 |
| Asian                                                             |     1 |
| cis female                                                        |     1 |
| Demale                                                            |     1 |
| demigirl                                                          |     1 |
| emale                                                             |     1 |
| famela                                                            |     1 |
| feamle                                                            |     1 |
| fem                                                               |     1 |
| femae                                                             |     1 |
| FEMAIIL                                                           |     1 |
| Femaile                                                           |     1 |
| femake                                                            |     1 |
| Femal                                                             |     1 |
| FEMAL                                                             |     1 |
| femal3                                                            |     1 |
| femald                                                            |     1 |
| FEMale                                                            |     1 |
| FEMALE                                                            |     1 |
| female (Cisgender)                                                |     1 |
| Female (cisgender)                                                |     1 |
| Female to non-binary                                              |     1 |
| Femalee                                                           |     1 |
| Femalep                                                           |     1 |
| femals                                                            |     1 |
| femenina                                                          |     1 |
| Feminine                                                          |     1 |
| fenale                                                            |     1 |
| fmale                                                             |     1 |
| ftm                                                               |     1 |
| g                                                                 |     1 |
| G                                                                 |     1 |
| Gender is a social construct - I’m sexually female                |     1 |
| girl                                                              |     1 |
| Girl                                                              |     1 |
| mae                                                               |     1 |
| mael                                                              |     1 |
| maill                                                             |     1 |
| make                                                              |     1 |
| Malae                                                             |     1 |
| mALE                                                              |     1 |
| MAle                                                              |     1 |
| Male                                                              |     1 |
| Male.                                                             |     1 |
| Male(Sex, Gender is a silly construct)                            |     1 |
| males                                                             |     1 |
| MALR                                                              |     1 |
| Man/Male                                                          |     1 |
| maoe                                                              |     1 |
| masculino                                                         |     1 |
| Mslr                                                              |     1 |
| nb                                                                |     1 |
| NB                                                                |     1 |
| Non binary                                                        |     1 |
| Non Binary                                                        |     1 |
| non-binary                                                        |     1 |
| Non-binary                                                        |     1 |
| Trans man                                                         |     1 |
| transgender                                                       |     1 |
| transgender female                                                |     1 |
| Transgender man                                                   |     1 |
| transmale                                                         |     1 |
| transmasculine                                                    |     1 |
| Transsexual male (FTM)                                            |     1 |
| woman                                                             |     1 |

Summary of gender categories before coding {.table}

Recoding using the `gender_coder()` function classifies all but 28
responses into pre-defined response categories.

``` r

manylevels_sample <- sample
manylevels_sample$recoded_gender <- recode_gender(
  gender = manylevels_sample$Gender,
  dictionary = manylevels_en
)

knitr::kable(
  head(manylevels_sample, 10),
  caption = "The manylevels_en dictionary applied to `head(sample)`"
)
```

| Gender | recoded_gender |
|:-------|:---------------|
| FEMALE | woman          |
| FEMAL  | woman          |
| male   | man            |
| FEMALE | woman          |
| female | woman          |
| male   | man            |
| feamle | woman          |
| male   | man            |
| male   | man            |
| male   | man            |

The manylevels_en dictionary applied to `head(sample)` {.table}

``` r

knitr::kable(
  count_values(na.omit(manylevels_sample$recoded_gender), "recoded_gender"),
  caption = "Summary of gender categories after use of the *manylevels_en* dictionary"
)
```

| recoded_gender    | count |
|:------------------|------:|
| woman             |  4015 |
| man               |  3692 |
| non-binary        |     8 |
| transgender man   |     4 |
| cis woman         |     3 |
| girl              |     2 |
| agender           |     1 |
| androgynous       |     1 |
| female            |     1 |
| transgender       |     1 |
| transgender woman |     1 |

Summary of gender categories after use of the *manylevels_en* dictionary
{.table}

In this dataset unclassified responses are a mix of unusual responses
and apparent response errors (e.g. numbers and symbols). While some of
these are genuinely missing (i.e. Gender = 40), other could be manually
recoded, or added to a custom dictionary.

``` r

knitr::kable(
  manylevels_sample[is.na(manylevels_sample$recoded_gender), ],
  caption = "All responses not classified by the built-in dictionary"
)
```

|  | Gender | recoded_gender |
|:---|:---|:---|
| 133 | 40 | NA |
| 391 | Gender | NA |
| 553 | agender (woman) | NA |
| 664 | masculino | NA |
| 1750 | Male. | NA |
| 1760 | 54 | NA |
| 1887 | % | NA |
| 2221 | Female to non-binary | NA |
| 2301 | Asian | NA |
| 2667 | masculino | NA |
| 2723 | masculino | NA |
| 3240 | masculino | NA |
| 3355 | masculino | NA |
| 3414 | Man/Male | NA |
| 3447 | demigirl | NA |
| 3519 | femenina | NA |
| 3544 | masculino | NA |
| 3570 | Masculino | NA |
| 4430 | transmasculine | NA |
| 4536 | Gender | NA |
| 4861 | Gender is a social construct - I’m sexually female | NA |
| 6053 | Apache Helicopter… Just kidding. There are only two. I am a Male. | NA |
| 6170 | masculino | NA |
| 6478 | masculino | NA |
| 6898 | Male(Sex, Gender is a silly construct) | NA |
| 7314 | Transsexual male (FTM) | NA |
| 7656 | Masculino | NA |

All responses not classified by the built-in dictionary {.table
style="width:100%;"}

### Options within the function

#### dictionary

The package provides two built-in dictionaries. The use of these is
controlled using the `dictionary` argument. The first
`dictionary = manylevels_en` provides corrects spelling and standardises
terms while maintaining the diversity of responses. This is the default
dictionary for
[`recode_gender()`](https://docs.ropensci.org/gendercoder/reference/recode_gender.md)
as it preserves as much gender diversity as possible.

However in some cases you may wish to collapse gender into a smaller set
of categories by using the `fewlevels_en` dictionary
(`dictionary = fewlevels_en`). This dictionary contains fewer gender
categories, “man”, “woman”, “boy”, “girl”, and “sex and gender diverse”.

The “man” category includes all participants who indicate that they are

- male
- trans male (including female to male transgender respondents)
- cis male

The “woman” category includes all participants who indicate that they
are

- female
- trans female (including male to female transgender respondents)
- cis female

The “sex and gender diverse” category includes all participants who
indicate that they are

- agender
- androgynous
- intersex
- non-binary
- gender-queer

``` r

fewlevels_sample <- sample
fewlevels_sample$recoded_gender <- recode_gender(
  gender = fewlevels_sample$Gender,
  dictionary = fewlevels_en
)

knitr::kable(
  head(fewlevels_sample, 10),
  caption = "The fewlevels_en dictionary applied to `head(sample)`"
)
```

| Gender | recoded_gender |
|:-------|:---------------|
| FEMALE | woman          |
| FEMAL  | woman          |
| male   | man            |
| FEMALE | woman          |
| female | woman          |
| male   | man            |
| feamle | woman          |
| male   | man            |
| male   | man            |
| male   | man            |

The fewlevels_en dictionary applied to `head(sample)` {.table}

``` r

knitr::kable(
  count_values(fewlevels_sample$recoded_gender, "recoded_gender"),
  caption = "Summary of gender categories after use of the *fewlevels_en* dictionary"
)
```

| recoded_gender         | count |
|:-----------------------|------:|
| woman                  |  4020 |
| man                    |  3696 |
| NA                     |    27 |
| sex and gender diverse |    11 |
| girl                   |     2 |

Summary of gender categories after use of the *fewlevels_en* dictionary
{.table}

You can also specify a custom dictionary to replace or supplement the
built-in dictionary. The custom dictionary should be a list in the
following format.

``` r

# name of the vector element is the user input value and the vector element is the 
# replacement value corresponding to that name as a lower case string.
custom_dictionary <- c(
  masculino = "man",
  hombre = "man",
  mujer = "woman",
  femenina = "woman"
)

str(custom_dictionary)
```

    ##  Named chr [1:4] "man" "man" "woman" "woman"
    ##  - attr(*, "names")= chr [1:4] "masculino" "hombre" "mujer" "femenina"

Custom dictionaries can be used in place of a built-in dictionary or can
supplement the built-in dictionary by providing a vector of vectors to
the dictionary argument. Where the lists contain duplicated elements,
the last version of the duplicated value will be used for recoding. This
allows you to use the built-in dictionary but change the coding of one
or more responses from that dictionary. Here the addition of Spanish
terms allows for recoding of 11 previously uncoded responses.

``` r

combined_sample <- sample
combined_sample$recoded_gender <- recode_gender(
  gender = combined_sample$Gender,
  dictionary = c(fewlevels_en, custom_dictionary)
)

knitr::kable(
  count_values(combined_sample$recoded_gender, "recoded_gender"),
  caption = "Summary of gender categories after use of the combined dictionaries"
)
```

| recoded_gender         | count |
|:-----------------------|------:|
| woman                  |  4021 |
| man                    |  3706 |
| NA                     |    16 |
| sex and gender diverse |    11 |
| girl                   |     2 |

Summary of gender categories after use of the combined dictionaries
{.table}

``` r

knitr::kable(
  combined_sample[is.na(combined_sample$recoded_gender), ],
  caption = "All responses not classified by the combined dictionaries"
)
```

|  | Gender | recoded_gender |
|:---|:---|:---|
| 133 | 40 | NA |
| 391 | Gender | NA |
| 553 | agender (woman) | NA |
| 1750 | Male. | NA |
| 1760 | 54 | NA |
| 1887 | % | NA |
| 2221 | Female to non-binary | NA |
| 2301 | Asian | NA |
| 3414 | Man/Male | NA |
| 3447 | demigirl | NA |
| 4430 | transmasculine | NA |
| 4536 | Gender | NA |
| 4861 | Gender is a social construct - I’m sexually female | NA |
| 6053 | Apache Helicopter… Just kidding. There are only two. I am a Male. | NA |
| 6898 | Male(Sex, Gender is a silly construct) | NA |
| 7314 | Transsexual male (FTM) | NA |

All responses not classified by the combined dictionaries {.table
style="width:100%;"}

#### retain_unmatched

The `retain_unmatched` argument is used to determine the handling for
recoding of values not contained in the dictionary. By default,
unmatched values are coded as NA. `retain_unmatched = TRUE` will fill
unmatched responses with the participant provided response.

``` r

retained_sample <- sample
retained_sample$recoded_gender <- recode_gender(
  gender = retained_sample$Gender,
  dictionary = c(fewlevels_en, custom_dictionary),
  retain_unmatched = TRUE
)
```

    ## Results not matched from the dictionary have been filled with the user inputted values

``` r

knitr::kable(
  count_values(retained_sample$recoded_gender, "recoded_gender"),
  caption = "Summary of gender categories after use of the combined dictionary and `retain_unmatched = TRUE`"
)
```

| recoded_gender                                                    | count |
|:------------------------------------------------------------------|------:|
| woman                                                             |  4021 |
| man                                                               |  3706 |
| sex and gender diverse                                            |    11 |
| Gender                                                            |     2 |
| girl                                                              |     2 |
| %                                                                 |     1 |
| 40                                                                |     1 |
| 54                                                                |     1 |
| agender (woman)                                                   |     1 |
| Apache Helicopter… Just kidding. There are only two. I am a Male. |     1 |
| Asian                                                             |     1 |
| demigirl                                                          |     1 |
| Female to non-binary                                              |     1 |
| Gender is a social construct - I’m sexually female                |     1 |
| Male.                                                             |     1 |
| Male(Sex, Gender is a silly construct)                            |     1 |
| Man/Male                                                          |     1 |
| transmasculine                                                    |     1 |
| Transsexual male (FTM)                                            |     1 |

Summary of gender categories after use of the combined dictionary and
`retain_unmatched = TRUE` {.table}

## A disclaimer on handling gender responses

This package attempts to remove typographical errors from free text
gender data. The defaults that we used are specific to our context and
the time at which the package was developed and your data or context may
be different.

We offer two built-in dictionaries, manylevels_en and fewlevels_en. Both
are necessarily opinionated about how gender descriptors collapse into
categories.

However, as these are culturally specific, they may not be suitable for
your data. In particular the fewlevels_en option makes opinionated
choices about some responses that we want to acknowledge are potentially
problematic. Specifically,

- In ‘fewlevels_en’ coding intersex responses are recoded as ‘sex and
  gender diverse’
- In ‘fewlevels_en’ responses where people indicate they are trans and
  indicate their identified gender are recoded as the identified gender
  (e.g. ‘Male to Female’ is recoded as ‘woman’). We wish to acknowledge
  that this may not reflect how some individuals would classify
  themselves when given these categories and in some contexts may make
  systematic errors. The manylevels_en coding dictionary attempts to
  avoid these issues as much as possible - however users can provide a
  custom dictionary to add to or overwrite our coding decisions if they
  feel this is more appropriate. We welcome people to update the
  built-in dictionary where desired responses are missing.
- In both dictionaries, we assume that typographical features such as
  spacing are not relevant to recoding the gender response (e.g. we
  assume that “genderqueer” and “gender queer” are equivalent). This is
  unlikely to be true for all contexts.

The ‘manylevels_en’ coding separates out those who identify as trans
female/male or cis female/male into separate categories it should not be
assumed that all people who describe as male/female are cis, if you are
assessing trans status we recommend a two part question see:

Bauer, Greta & Braimoh, Jessica & Scheim, Ayden & Dharma, Christoffer.
(2017). Transgender-inclusive measures of sex/gender for population
surveys: Mixed-methods evaluation and recommendations. PLoS ONE. 12.

## Contributing to this package

This package is a reflection of cultural context of the package
contributors. We acknowledge that understandings of gender are bound by
both culture and time and are continually changing. As such, we welcome
issues and pull requests to make the package more inclusive, more
reflective of current understandings of gender inclusive languages
and/or suitable for a broader range of cultural contexts. We
particularly welcome addition of non-English dictionaries or of other
gender-diverse responses to the manylevels_en and fewlevels_en
dictionaries.

The “Adding to the dictionary” vignette includes information about how
to make changes to the dictionary either for your own use or when
contributing to the gendercoder package.

## Acknowledgement of Country

We acknowledge the Wurundjeri people of the Kulin Nation as the
custodians of the land on which this package was developed and pay
respects to elders past, present and future.
