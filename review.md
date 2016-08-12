@jsta @sckott here's my review.  See the comments below for items that aren't checked.

- [x] As the reviewer I confirm that there are no conflicts of interest for me to review this work (such as being a major contributor to the software).

#### Documentation

The package includes all the following forms of documentation:

- [ ] **A statement of need** clearly stating problems the software is designed to solve and its target audience in README
- [x] **Installation instructions:** for the development version of package and any non-standard dependencies in README
- [ ] **Vignette(s)** demonstrating major functionality that runs successfully locally
- [ ] **Function Documentation:** for all exported functions in R help
- [ ] **Examples** for all exported functions in R Help that run successfully locally
- [ ] **Community guidelines** including contribution guidelines in the README or CONTRIBUTING, and `URL`, `Maintainer` and `BugReports` fields in DESCRIPTION 

#### Functionality

- [x] **Installation:** Installation succeeds as documented.
- [x] **Functionality:** Any functional claims of the software been confirmed.
- [x] **Performance:** Any performance claims of the software been confirmed.
- [x] **Automated tests:** Unit tests cover essential functions of the package
   and a reasonable range of inputs and conditions. All tests pass on the local machine.
- [x] **Packaging guidelines**: The package conforms to the rOpenSci packaging guidelines

#### Final approval (post-review)

- [ ] **The author has responded to my review and made changes to my satisfaction. I recommend approving this package.**

Estimated hours spent reviewing: 3

---

### Review Comments

This package provides access to water quality and hydrological data from the dbhydro database provided by the South Florida Water Management District.  Although the package has a regional focus, the database could be used to address more general ecological or environmental management questions.  Having a package to more easily acquire the database will no doubt facilitate use and reach of the data.  

The package is lightweight and is built around two main functions.  The documentation and examples could be improved in some cases (see below), but overall I do not have any major concerns about package structure or functionality.  However, access to metadata for the database is a serious limitation that needs to be addressed.  Sure I could go online to find what I need but it seems like the package should facilitate that process in some way.  The package does a good job retrieving data if the station id and test name are known but it would be much more helpful if there was some way to query the available information.  Here are some options to consider:

* The vignette has an example of retrieving data using a wild card for the station id.  Some more examples like this would be very helpful.
* Including more detailed metadata in the appendix of the vignette, e.g., stations, date ranges, etc.
* Including more functions to return and search metadata.  My SWMPr package has similar functions (e.g., `SWMPRr::site_codes` or `SWMPr::site_codes_ind`) but I have the advantage of much fewer stations than those in dbhydro, so I'm not sure the best approach.  

#### Build/install 

* I was having problems building the vignette when running `devtools::check()`.  The file `dbhydroR.tex` was not compiling and returning an error "support pakage l3kernel too old".  Updating the package in my LaTeX distribution fixed the issue.  I'm guessing you didn't have this issue since the package is already on CRAN but I wanted to mention it in case the problem occurs in the future.  Otherwise, the devtools build on my Windows machine and the CRAN Windows builder (`devtools::build_win()`) passed with no notes, warnings, or errors.   

#### Examples

* The following error was returned running `devtools::run_examples(pkg = '.', run = FALSE)`):
```{r}
> cleanhydro(gethydro(dbkey = "15081", date_min = "2013-01-01", date_max = "2013-02-02"))
Error: is.response(x) is not TRUE
```
I ran the check to include examples in `\dontrun` tags, as for the above. It makes sense to not run the example if it's supposed to fail but there is no description exaplaining what the example shows.  Either fix the example above or provide some explanation as to why it fails.  All other examples ran without issues.   

#### Documentation/vignette

* Maybe you should include some details (`@details`) for `getwq` and `gethydro`.  The documentation for each is minimal, so perhaps add some of the text in the vignette to the help files for each function.     

* As noted above under documentation requirements, the README does not include a 'statement of need clearly stating problems the software is designed to solve and its target audience'.  The text from the onboarding issue above would be a good addition, i.e., '`dbhydroR` provides scripted access to the South Florida Water Management District's DBHYDRO database which holds over 35 million water quality and hydrologic data records from the Florida Everglades and surrounding areas. The target audience is anyone interested in water quality and hydrologic data from the Florida Everglades - ecologists, engineers, meteorologists, hydrogeologists, hydrologists, etc.'

* For the vignette (or even on the README), I think it would be useful to provide a more user-friendly view of the available stations for `getwq`.  The link in the vignette is to a KMZ file, which is not easily viewed.  I tried installing the KML/KMZ Viewer for Chrome but it was blocked on my goverment computer, so I suspect others would have problems.  Another option is to include an attachment to the vignette with a table or map showing the stations.  

#### Functions

* I'm wondering if `cleanhydro` and `cleanwq` should be exported since they are only used within `getwq` and `gethydro`.  I say this because the documentation is sparse (no details, no returned value description, minimal examples).  Maybe it's better not to export.  Would a user ever have a separate need for these functions?  

* For `cleanwq`, the vignette says that rows with missing data are removed.  What happens with these rows if multiple stations or 'test_names' are queried?  Is the entire row removed even if data are available for the other columns?  I think some clarification is needed.

#### Contributing

* Might be worth including a `contributing.md file` on the repo, or something more minimal in the README.  Maybe modify [this](https://github.com/USEPA/R-micromap-package-development/blob/master/CONTRIBUTING.md) for your needs?  

#### Compliance with rOpenSci Packaging Guide

My comments above address most of the compliance concerns for rOpenSci.  Below are some minor, additional points.

##### Function variable naming

I guess the CRAN gatekeepers don't care about this but I was having a mental block with `getwq` given it's similarity to `getwd`.  You might consider changing the function name to make it more distinct. 

##### README

The ropensci footer will have to be added eventually: `[![ropensci_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)`

##### Code of conduct

Also consider adding the code of conduct badge with `devtools::use_code_of_conduct`. 

Hopefully these comments are helpful.  Let me know if you have any questions. 