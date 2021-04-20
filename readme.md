-   [YAML metadata (header) in R
    Markdown](#yaml-metadata-header-in-r-markdown)
-   [Useful websites](#useful-websites)
-   [useful tools](#useful-tools)
-   [Terminology and norms](#terminology-and-norms)
-   [Basic](#basic)
    -   [Syntax](#syntax)
    -   [Basic setting metadata](#basic-setting-metadata)
-   [advanced options for html and
    pdf](#advanced-options-for-html-and-pdf)
    -   [html](#html)
    -   [pdf](#pdf)
-   [style control](#style-control)
    -   [html](#html-1)
    -   [pdf](#pdf-1)
-   [Bibliographies and Citations](#bibliographies-and-citations)
-   [Chinese support](#chinese-support)
-   [params](#params)
-   [Updating…](#updating)

# YAML metadata (header) in R Markdown

Rmd makes it possible to use a YAML header to specify certain parameters
right at the beginning of the document. Built-in YAML parameters make it
easier to create more organized and informative reports. However, there
are few tutorials or summarized articles to display all the settings and
parameters for `YAML metadata` in R Markdown. This readme file give you
the power to easily control R Markdown by YAML.

# Useful websites

-   <https://rmarkdown.rstudio.com/formats.html> — R Markdown formats
    from RStudio
-   <https://www.r-bloggers.com/2020/08/useful-yaml-options-for-generating-html-reports-in-r/>
    — Useful YAML options for generating HTML reports in R

# useful tools

-   <https://ymlthis.r-lib.org/index.html> — ymlthis: a package for
    writing YAML for R Markdown

-   <https://github.com/kamapu/yamlme> — This package aims to save
    documents with their respective settings (yaml-head) in R-objects.

# Terminology and norms

-   R lang – use upper case letter `R`
-   Rmd – capitalizes the first letter for `Rmd` files
-   R Markdown – the R Markdown module or notebook of Rstudio
-   rmarkdown – the package of `rmarkdown`
-   YAML – /ˈjæməl/ a recursive acronym for “YAML Ain’t Markup Language”
-   YAML header/metadata – the settings data written by YAML in Rmd
    header
-   All the YAML metadata are `lower case letters` except file paths or
    file names

# Basic

## Syntax

### Data Structure

Source: ymlthis package(Barrett and Iannone 2021) vignette

A YAML code block should be fenced in with `---` before and after (you
can also use … to end the YAML block, but this is not very common in R
Markdown).

1.  Scalars, or variables, are defined using a colon and a **space**.A
    dictionary is represented in a simples `key: value` form (the colon
    must be followed by a `space`)

        ---
        title: "YAML metadata for R Markdown with examples"
        author: Hao Liang
        fontsize: 12pt
        ---

2.  All members of a list are lines beginning at the same indentation
    level starting with a `'- '` (a dash and a space):

        # A list of tasty fruits
        - Apple
        - Orange
        - Strawberry
        - Mango

        # OR
        [Apple, Orange, Strawberry, Mango]

3.  More complicated data structures are possible, such as lists of
    dictionaries, dictionaries whose values are lists or a mix of both:

        author: 
         - Name_1   # can be indented or not
         - Name_2   # but be consistent among different entries

4.  Dictionaries and lists can also be represented in an abbreviated
    form if you really want to:

        author: [Name_1, Name_2]

5.  Strings can be denoted with a `|` character, which preserves
    newlines, or a `>` character, which folds newlines.

        abstract: |
          One or two sentences providing a **basic introduction** to the field,  comprehensible     to a scientist in any discipline.

          Two to three sentences of **more detailed background**, comprehensible  to scientists     in related disciplines.


        abstract: >
          One or two sentences providing a **basic introduction** to the field,  comprehensible     to a scientist in any discipline.

          Two to three sentences of **more detailed background**, comprehensible  to scientists     in related disciplines.

6.  Logical values in YAML are unusual: `true/false`, `yes/no`, and
    `on/off` are all equivalent to TRUE/FALSE in R. Any of these turn on
    the table of contents:

        toc: true
        toc: yes
        toc: on

### Indent

In YAML, spaces(indent) are used to indicate nesting (`tab` is not
recommended.). When we want to specify the output function
`pdf_document(toc = TRUE)`, we need to nest it under the `output` field.
We also need to nest `toc` under pdf\_document so that it gets passed to
that function correctly.

    ---
    output:
      pdf_document:
        toc: true
    ---

In R, the equivalent structure is a nested list, each with a name:
`list(output = list(pdf_document = list(toc = TRUE)))`. Similarly, you
can call this in R Markdown using the metadata object,
e.g. `metadata$output$pdf_document$toc`. The hierarchical structure
(which you can see with `draw_yml_tree()`) looks like this:

    └── output:
        └── pdf_document:
            └── toc: true

Without the extra indents, YAML doesn’t know toc is connected to
`pdf_document` and thinks the value of `pdf_document` is NULL. YAML that
looks like this:

    output:
      pdf_document:
    toc: true

    ├── output:
    │   └── pdf_document: null
    └── toc: true

Some YAML fields take unnamed vectors as their value. You can specify an
element of the vector by adding a new line and - (note that the values
can be indented or not below category here).

    category:
    - R
    - Reprodicible Research

### quote and R code

You may have noticed that strings in YAML don’t always need to be
quoted. However, it can be useful to explicitly wrap strings in quotes
when they contain special characters like : and @.

    title: 'R Markdown: An Introduction'

R code can be written as inline expressions `` `r expr` ``. `yml_code()`
will capture R code for you and put it in a valid format. R code in
`params` needs to be slightly different: use `!r`(e.g. `!r expr`) to
call an R object.

    author: 'liang'
    params:
      date: !r Sys.Date()

## Basic setting metadata

### Top-level basic settings

These settings are based on the original `rmarkdown` package without any
other associated packages.

Set Top-level Basic R Markdown YAML Fields. e.g.

    ---
    title: "YAML metadata for R Markdown with examples"
    subtitle: "YAML header"
    author: Hao Liang
    date: "2021-04-20"
    output:
      md_document:
        toc: yes
        toc_depth: 2
    abstract: YAML is a human-readable and easy to write language to define data structures.
    keywords: ["YAML", "Rmd"]
    subject: Medicine
    description: Rmd makes it possible to use a YAML header to specify certain parameters right at the beginning of the document.
    category: 
     - Rmd
     - Medicine
    lang: "en-US" 
    ---

This field is not available in all output formats. It is available in:

<table>
<colgroup>
<col style="width: 13%" />
<col style="width: 15%" />
<col style="width: 14%" />
<col style="width: 15%" />
<col style="width: 14%" />
<col style="width: 26%" />
</colgroup>
<thead>
<tr class="header">
<th>field</th>
<th>html_document</th>
<th>pdf_document</th>
<th>word_document</th>
<th>odt_document</th>
<th>powerpoint_presentation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>subtitle</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td>abstract</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td>keywords</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
</tr>
<tr class="even">
<td>subject</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
</tr>
<tr class="odd">
<td>description</td>
<td></td>
<td></td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
</tr>
<tr class="even">
<td>category</td>
<td></td>
<td></td>
<td>:smiley:</td>
<td></td>
<td>:smiley:</td>
</tr>
<tr class="odd">
<td>lang</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
<td>:smiley:</td>
</tr>
</tbody>
</table>

The document language using IETF language tags such as “en” or “en-US.”
The [language subtag lookup tool](https://r12a.github.io/app-subtags/)
can help find the appropriate tag.

> Certain R Markdown templates will allow you to specify additional
> parameters directly within the YAML. For example, the Distill output
> format allows url, affiliation, and affiliation\_url to be specified.
> After you install the distill package. You can see the corresponding
> section in this tutorial.

    ---
    title: "Distill for R Markdown"
    author:
      - name: "JJ Allaire"
        url: https://github.com/jjallaire
        affiliation: RStudio
        affiliation_url: https://www.rstudio.com
    output: distill::distill_article
    ---

### output

The `output` field of YAML is very important for R Markdown. The
`rmarkdown` package contains a lot of output formats for different use.

Source: <https://rmarkdown.rstudio.com/lesson-9.html>

    ---
    output: html_notebook
    ---

The `rmarkdown` package/ Rmd natively support the following formats:

-   html\_notebook
-   html\_document
-   pdf\_document
-   word\_document
-   odt\_document
-   rtf\_document
-   md\_document
-   ioslides\_presentation
-   beamer\_presentation
-   powerpoint\_presentation
-   html\_vignette

If you use one output format without additional arguments, the value of
output can simply be the name of the function.

    ---
    output: html_document
    ---

However, if you’re specifying more than one output type, you must use
the nesting syntax. If you don’t want to include additional arguments,
use “default” as the function’s value.

    ---
    output:
      html_document: default
      pdf_document: default
    ---

### toc

`toc` is the sub-level option of any output formats. You can add a table
of contents (TOC) using the `toc` option and specify the depth of
headers that it applies to using the `toc_depth` option(Xie 2018). For
example:

    ---
    title: "Habits"
    output:
      html_document:
        toc: true
        toc_depth: 2
        toc_float: # invalid for other output formats of non-html
          collapsed: false
          smooth_scroll: false
    ---

# advanced options for html and pdf

![source:<https://yongfu.name/2019-fju-rmd-talk/slide/#1>](images/htmlvpdf.png)

## html

## pdf

# style control

## html

## pdf

### header-includes:

# Bibliographies and Citations

<https://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html>

# Chinese support

# params

# Updating…

Barrett, Malcolm, and Richard Iannone. 2021. *Ymlthis: Write ’YAML’ for
’r Markdown’, ’Bookdown’, ’Blogdown’, and More*.
<https://CRAN.R-project.org/package=ymlthis>.

Xie, Yihui. 2018. *R Markdown: The Definitive Guide*.
<https://doi.org/10.1201/9781138359444>.
