# papaja 0.1.0.9492

### Template
- The ampersand filter is applied with `--vanilla` to avoid problems with customized `.Rprofile`s (reported by @awellis, #96)
- Fixed bug in `ampersand_filter()` regex, which caused some citations to be missed depending on preceeding citations (reported by @awellis, #96, and @m-Py, #100)
- On windows the Batch file to apply the ampersand filter is now created at the current location and deleted on exit to avoid permission problems (reported by @stahl-c)
- Changed default graphics devices to `pdf` and `png` to improve compatibility and rendering speed; `postscript` and `tiff` are no longer used be default
- Author note is omitted if no corresponding author is defined and no author note text is provided (reported by @jacob-long, #108; fixed by @jacob-long, #109)

### Existing functions
- `apa_print.aov()`
    - Fixed bug that caused incorrect calculation of eta squared
    - MSE is now typeset in regular font rather than italic as per APA guidelines
- `apa_table()`
    - Added proper support for automated bookdown cross-referecing of tables: Chunk label is now automatically added als `\label{}` tag.
    - Now supports `na_string` and `digits` options
- `theme_apa()`
    - Now uses `ggplot::margin()` to set margins because `ggplot::unit()` is now depricated.
    - Changed base size default from 14 to 12 and adjusted some margin and spacing defaults
- `r_refs()`
    - Fixed bug that caused some existing references to be added to the bib-file (#102)
- `cite_r()`
    - Fixed incorrect handling of `survival-book` reference

### New functions
- `apa_print.lsmobj()`
- `apa_print.summary.ref.grid()`
- `apa_print.glht()`
- `apa_print.summary.glht()`

### Misc
- New global option `papaja.na_string` with default value `"NA"` used by `apa_table()` and `printnum()`
- Updated example file and README


# papaja 0.1.0.9470

### Template
- The template now automatically invokes an R-based pandoc filter that replaces `&` with `and` in in-text citations to fully adhere to APA citation guidelines; (see #53)
- Initial chunk in template now is set to use chunk option `include = FALSE`.

### Existing functions
- `apa_table.latex()`
    - If `landscape = TRUE` tables are now always in `longtable` environments to improve interplay with the LaTeX package `endfloat` and correctly honoring the `figsintext` option in the YAML header (reported by @m-Py, #90)
    - Added clarification that the chunk option `results = "asis"` is required when `apa_table()` is called (requested by 	RMHogervorst, #93)
- `apa_print.lm()`
    - Calling `lm()` on a `data.frame` columns by using `$` resulted in regression term names that broke `apa_table()` (reported by @m-Py, #87)
- `render_appendix()`
    - Now builds on `knitr::knit_child()` to fix a bug causing figures not to render in the appendix; `render_appendix()` now has to be called within an R Markdown document. (reported by @fdabl, #70)
- `apa_*plot()`
    - Expressions can now be used as legend titles (see #94)


# papaja 0.1.0.9456

### Template
- `apa6_pdf` and `apa6_word` now adapt `pdf_document2()` and `word_document2()` from the `bookdown` package. This enables the use of `bookdown` cross-referencing syntax including automatically generated table and figure labels as detailed [here](https://bookdown.org/yihui/bookdown/cross-references.html) (see #2).
- LaTeX
    - If `numbersections: true` numbering now starts at the body of the text. The abstract is no longer a numbered heading (reported by @lnalborczyk, #85)
    - New option `footnotelist` to create list of footnotes at the end of the document (see #74)
- Word
    - Fixed bug that the wordcount set in the YAML header was not reported on the title page of Word documents (reported by @TobiasKoeln)

### Existing functions
- `apa_table.latex()`
    - Fixed bug causing longtables and and landscape tables to appear in text rather than at the end of the document when `figsintext: false` (reported by @m-Py, #64).
    - Fixed bug causing LaTeX to break when table contains `%`. All unescaped `%` are now automatically escaped even if `escape = FALSE` (reported by @m-Py, #66).

### Vignette
- Corrected various typos (reported by JooYoung Seo)


# papaja 0.1.0.9440

### Template
- Fixed bug in inline hook that omitted single numbers (reported by @hcp4715, #79)
- Chunk option default changed to `results = "markup"` (see #77)

### Existing functions
- `apa_barplot()` can calculate `ylim` when measure of dispersion cannot be calculated (#76)
- `apa_beeplot()` brightens data points instead of using transparency
- `apa_print()`
    - Renamed returned list elements to `statistic`, `estimate`, `full_result`, and `table` (#67)
    - All methods now return lists of the same structure (`table` may be `NULL`)
- `apa_print.lm()` has new option `observed_predictors` to calculate proper R^2 confidence intervals


### Misc
- MBESS package is now suggested and not imported

# papaja 0.1.0.9423

### Template
- Fixed a bug causing a LaTeX error in `jou` mode (reported by @saladspices, #48)
- Default `lang` is now `english`; fixed a pandoc-citeproc bug (reported by @mvuorre and @sebastiansauer, #56 and #57; fixed by @mvuorre, #58)
- Removed `author` field `department`, information can be supplied as part of the `address` field
- New option `author_note`; `note` is used to add additional information to the title page
- Changed heading "Sample" to "Participants"
- Fixed bug that supplied CSL file was ignored
- Proper author name concatination on title page (reported by Ulf Mertens)
- LaTeX
    - New options `figurelist` and `tablelist` to create lists of figure or table captions at the end of the document
    - Added `upgreek` package
    - Fixed bug causing `figsintext` to have no effect (reported by @mattsigal, #61)
    - Added page break before reference section (reported by @mattsigal, #60)
- Word
    - Fixed bug causing a deranged title page (reported by @mvuorre, #59)
    - Figure captions are now preceded by "Figure."

### Existing functions
- `apa_print()`
    - Improved type setting of predictors in tables
    - New supports `car::LeveneTest()` output
    - Dropped redundant df-column in `apa_print.lm()` table
    - Fixed bug causing the omission of R^2 confidence intervalls in `apa_print.lm()` when F-values were small
    - Fixed bug in `apa_print.list()` causing an error with only two models
    - Refactored `apa_print()`-family for ANOVA objects
        - Only df corrected for nonsphericity are reported with two decimals
        - Added indication of sphericity correction to `apa_print.Anova.mlm()` table
        - `print_anova()` rounds effect size estimates to three decimals
        - Can now calculate eta^2 effect sizes
        - New option `intercept` determines if intercept tests are included in the output
        - New option `mse` to report mean squared error terms (thanks to @TomHardwicke for reporting early bugs, #49)
- `apa_table()`
    - Dropped redundant `row_names` option
    - New option `stub_indents` adds title sections to indented rows
    - Renamed option `added_colnames` to `added_stub_head`
    - `apa_table.latex()`
        - New option `col_spanners` to add column spanners
        - New option `small` to decrease font size for large tables
        - Improvements to long and landscape tables
- `apa_barplot()`
    - Now works with white space in factor names
    - Improvements to automatic setting of `ylim`
    - Fixed bug that led to the incorrect drawing of user-specified colors
    - Only one legend is drawn for multiple facets
    - New option `args_arrows` to customize error bars
    - Invisibly returns means and measures of dispersion
- `cite_r()`
    - New options `pkgs` and `withhold` to black- or whitelist packages (#50)
    - Output retains structure if footnote = TURE but no bib-file is found
- `r_refs()`
    - Now handles packages with mulitple references (reported by @mattsigal, #65)
- `conf_int()`
    - Fixed bug that led to negative values

### New functions
- `apa_print.afex_aov()` (#47)
- `apa_print.list()` (#27)
- `wsci()`
- `apa_lineplot()`
- `apa_beeplot()` (thanks to @TobiasKoeln for reporting early bugs, #72)
- `theme_apa()`
- `render_appendix()`
- `fetch_zotero_refs()`
- `in_paren()`

### Dropped functions
- `fetch_web_refs()` evolved into a clone of `downloder::download()`
- `apa_prepare_doc()` is no longer needed

### Misc
- papaja sets global options `papaja.mse` and `papaja.sphericity_correction` among others
- Updated example manuscript
- Added vignette


# papaja 0.1.0.9054

### Template
- Added `wordcount` parameter to YAML header
- Changed YAML header structure to support proper author and affiliation information (requires updates of existing Rmd-files)
- LaTeX: Changed SVG device to PS
- Removed author field 'department' from the YAML header
- Added LaTeX fix for tightlist-bug in pandoc 1.14
- Fixed extra comma in automatically generated corresponding author line when no note is provided

### Existing functions
- `apa_print()`
    - Added `table` output-element to `apa_print.lm()` containing complete regression table
    - Refactored and tested `apa_print()`-family for ANOVA objects
    - Added `table` output-element to `apa_print()`-family for ANOVA objects containing complete ANOVA table
    - First experimental implementation of F-test model comparisons (`car::leveneTest` objects)
    - Estimates in `apa_print.htest()` and `apa_print.lm()` can be formatted by passing options to `printnum()`
- `apa_table()`
    - Added `midrules` option for `apa_table.latex()` and cleaned up LaTeX output
    - Fixed vertical space between table and table notes for `apa_table.latex()`
    - Bug fixes
- `r_refs()`
    - Now attempts to create references for packages used in cached chunks
    - Creates proper references based on citation file if available instead of generating default references from description file
- `apa_prepare_doc()`
    - Fixed bug when `lang` was not set in YAML header
- `apa_barplot()`
    - Fixed ignorance of user specified color for one factorial plots
    - Fixed default ylim choice to consider the lower bound of confidence intervals
- `apa_table()`
    - Removed redundant `row_name` parameter
    - Fixed escaped multicolumn-headings in `apa_table.latex()`

### New functions
- `apa_print.anova.mlm()`
- `apa_barplot()`
- `cite_r()`
- `fetch_web_refs()`

### Misc
- Set up proper package structure for automatic unit testing
- Created news file
- Fixed some typos in the example document

# papaja 0.1.0.9000

- Initial release on GitHub
