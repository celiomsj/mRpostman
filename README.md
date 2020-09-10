
<!-- This document must be rendered in RStudio using the option "knitr with parameters" or rmarkdown::render("MyDocument.Rmd", params = list(password = "my_password"))-->

<!-- README.md is generated from README.Rmd. Please edit .Rmd file -->

# mRpostman <img src="man/figures/logo.png" align="right" width="140" />

<!-- # mRpostman <img src="man/figures/logo.png" align="right" /> -->

<!-- [![Downloads](http://cranlogs.r-pkg.org/badges/mRpostman?color=brightgreen)](http://www.r-pkg.org/pkg/mRpostman) -->

<!-- one space after links to display badges side by side -->

[![Travis-CI Build
Status](https://travis-ci.org/allanvc/mRpostman.svg?branch=master)](https://travis-ci.org/allanvc/mRpostman)
[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/mRpostman)](https://cran.r-project.org/package=mRpostman)
[![Downloads from the RStudio CRAN
mirror](https://cranlogs.r-pkg.org/badges/mRpostman)](https://cran.r-project.org/package=mRpostman)
[![CRAN/METACRAN](https://img.shields.io/cran/l/mRpostman)](https://opensource.org/licenses/GPL-3.0)

IMAP Toolkit for R

## Overview

`mRpostman` provides tools for searching and fetching emails, mailbox
management, attachment extraction, and several other IMAP
functionalities. This package makes extensive use of {curl} and
{libcurl} to implement an easy-to-use IMAP client for R, paving the way
for users to perform data analysis on email data.

mRpostman’s official website: <https://allanvc.github.io/mRpostman>

**IMPORTANT**:

1.  In version `0.9.0.0`, `mRpostman` went trough thorough changes,
    including ones that have no backward compatibility with versions
    `<= 0.3.2`. A detailed vignette on how to migrate your mRpostman’s
    deprecated code to the new syntax is available at [*“Migrating old
    code to the new mRpostman’s
    syntax”*](https://allanvc.github.io/mRpostman/articles/code_migration.html)
    for more information.

2.  Old versions of the libcurl library will cause malfunctioning of
    this package. If your libcurl’s version is above 7.58.0, you should
    be fine. If you intend to use OAuth 2.0 authentication, then you
    will need libcurl \>= 7.65.0. To know more about the OAuth 2.0
    authentication in this package, refer to the [*“IMAP OAuth2.0
    authentication in
    mRpostman”*](https://allanvc.github.io/mRpostman/articles/xoauth2.0.html)

## First things first … (plain authentication)

Before using **mRpostman**, it is essential to configure access to your
email account. Many mail providers enabling the **“less secure apps”**
option to allow access from a third-party app, using plain
authentication. If you are interested in OAuth2.0 authentication, check
the [*“IMAP OAuth2.0 authentication in
mRpostman”*](https://allanvc.github.io/mRpostman/articles/xoauth2.0.html)
vignette.

Let’s see how to configure simple plain authentication for Gmail, Yahoo
Mail, AOL Mail, and Office 365.

### Gmail

1)  Go to the Gmail website and log in with your credentials.

2)  Then, go to
    <https://myaccount.google.com/u/1/lesssecureapps?pageId=none>

![](man/figures/gmail1.png) <!-- <img src="man/figures/gmail1.png"> -->

3)  Set “Allow less secure apps” to **ON**.

### Yahoo Mail

1)  Go to the Yahoo Mail website and log in with your credentials.

2)  Click on “Account Info”.

![](man/figures/yahoo1.png) <!-- <img src="man/figures/yahoo1.png"> -->

3)  Click on “Account Security” on the left menu.

![](man/figures/yahoo2.png) <!-- <img src="man/figures/yahoo2.png"> -->

4)  After, set “Allow apps that use less secure sign in” **ON**

![](man/figures/yahoo3.png)

<!-- <img src="man/figures/yahoo3.png"> -->

### AOL Mail

1)  Go to the AOL Mail website and log in with your credentials.

2)  Click on “Options” and then on “Account Info”.

![](man/figures/aol1.png) <!-- <img src="man/figures/aol1.png"> -->

3)  Click on “Account Security” on the left menu.

![](man/figures/aol2.png) <!-- <img src="man/figures/aol2.png"> -->

4)  After, set “Allow apps that use less secure sign in” **ON**

![](man/figures/aol3.png) <!-- <img src="man/figures/aol3.png"> -->

### Outlook - Office 365

There is no need to execute any external configuration. Please, notice
that the `url` parameter in `configure_imap()` should be set as `url =
"imaps://outlook.office365.com"`.

## Introduction

From version 0.9.0.0000 onward, `mRpostman` is implemented under the OO
paradigm, based on an R6 class called `ImapCon`, its derived methods,
and a few independent functions with the aim to perform a myriad of IMAP
commands.

The package is divided in 8 groups of operations. Below, we present all
the available methods/functions:

  - 1.  **configuration**: `configure_imap()`;

  - 2.  **server capabilities**: `list_server_capabilities()`

  - 3.  **mailbox operations**: `list_mail_folders()`,
        `select_folder()`, `examine_folder()`, `rename_folder()`,
        `create_folder()`;

  - 4.  **single search**: `search_before()`, `search_since()`,
        `search_period()`, `search_on()`,
        `search_sent_before()`,`search_sent_since()`,
        `search_sent_period()`, `search_sent_on()`, `search_string()`,
        `search_flag()`, `search_smaller_than()`,
        `search_larger_than()`, `search_younger_than()`,
        `search_older_than()`;

  - 5.  **custom search**: `search`
    
    <!-- end list -->
    
      - **custom search helper functions**:
          - relational operators functions: `AND()`, `OR()`;
          - criteria definition functions: `before()`, `since()`,
            `on()`, `sent_before()`, `sent_since()`, `sent_on()`,
            `string()`, `flag()`, `smaller_than()`, `larger_than()`,
            `younger_than()`, `older_than()`;

  - 6.  **fetch**: `fetch_body()`, `fetch_header()`, `fetch_text()`,
        `fetch_metadata()`, `fetch_attachments_list()`,
        `fetch_attachments()`;

  - 7.  **attachments**: `list_attachments()`, `get_attachments()`,
        `fetch_attachments_list()`, `fetch_attachments()`;

  - 8.  **complementary operations**: `copy_msg()`, `move_msg()`,
        `delete_msg()`, `expunge()`, `esearch_count()`,
        `esearch_min_id()`, `esearch_max_id()`, `add_flags()`,
        `remove_flags()`, `replace_flags()`;

## Installation

``` r
# CRAN version
install.packages("mRpostman")

# Dev version
if (!require('remotes')) install.packages('remotes')
remotes::install_github("allanvc/mRpostman")
```

## Basic Usage

### 1\) Configuring an IMAP connection and listing server’s capabilities

``` r

library(mRpostman)

# Outlook - Office 365
con <- configure_imap(url="imaps://outlook.office365.com",
                      username="your_user@company.com",
                      password=rstudioapi::askForPassword()
)

# other IMAP providers that were tested: Gmail (imaps://imap.gmail.com), 
#   Yahoo (imaps://imap.mail.yahoo.com/), AOL (imaps://export.imap.aol.com/),
#   Yandex (imaps://imap.yandex.com)

# you can try another IMAP server and see if it works

con$list_server_capabilities()
```

### 2\) Listing your mail folders and select “INBOX”

``` r

# Listing
con$list_mail_folders()

# Selecting
con$select_folder(name = "INBOX")
```

### 3\) Searching messages by date, with a flag as additional filter

``` r

res1 <- con$search_on(date_char = "02-Jan-2020")

res1
```

### 4\) Customizing a search with multiple criteria

``` r

# messages that contain either "@k-state.edu" OR "ksu.edu" in the "TO" header field
res2 <- con$search(OR(
  string(expr = "@k-state.edu", where = "TO"),
  string(expr = "@ksu.edu", where = "TO")
))

res2
```

### 5\) Fetch messages’ text using single-search results

``` r

res3 <- con$search_string(expr = "Welcome!", where = "SUBJECT") %>%
  con$fetch_text(write_to_disk = TRUE) # also writes results to disk

res3
```

## 6\) Attachments

You can list the attachments of one or more messages with:

1)  the `list_attachments()` function:

<!-- end list -->

``` r

con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_text() %>% # or with fetch_body()
  list_attachments() # does not depend on the 'con' object
```

… or more directly with:

2)  `fetch_attachments_list()`

<!-- end list -->

``` r

con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_attachments_list()
```

If you want to download the attachments of one or more messages, there
are two ways of doing that as well.

1)  Using the `get_attachments()` method:

<!-- end list -->

``` r

con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_text() %>% # or with fetch_body()
  con$get_attachments()
```

… and more directly with the

2)  `fetch_attachments()` method:

<!-- end list -->

``` r

con$search_since(date_char = "02-Jan-2020") %>%
  con$fetch_attachments()
```

## Future Improvements

  - add further IMAP functionalities;
  - eliminate the stringr dependency in REGEX;
  - implement a progress bar in fetch operations.

## Known bugs

  - *search results truncation*: This is a [libcurl’s known
    bug](https://curl.haxx.se/docs/knownbugs.html#IMAP_SEARCH_ALL_truncated_respon)
    which causes the search results to be truncated when there is a
    large number of message ids returned. To circumscribe this problem,
    you can set a higher `buffersize` value, increasing the buffer
    capacity, and `verbose = TRUE` for monitoring the server response
    for truncated results when executing a search. When possible,
    `mRpostman` tries to issue a warning for possible truncated values.

  - *`verbose = TRUE` malfunction on Windows*: This seems to be related
    to the [`curl` R
    package](https://github.com/jeroen/curl/issues/230). When using the
    `verbose = TRUE` on Windows, the flow of information between the
    IMAP server and the R session presents an intermittent behavior,
    which causes it to not be shown on the console, or with a
    considerable delay.

  - *shared mailbox access*: This seems to be another [libcurl’s
    bug](https://github.com/allanvc/mRpostman/issues/2), although more
    tests need to be done to confirm it. It does not allow the user to
    connect to a shared mailbox. To circumscribe this, if the shared
    mailbox has a password associated with it, you can try a direct
    regular connection.

  - *`xoauth2_bearer` SASL error*: This is related to [old libcurl’s
    versions](https://curl.haxx.se/bug/?i=2487) which causes the access
    token to not be properly passed to the server. This bug was fixed in
    libcurl 7.65.0. The problem is that many Linux distributions, such
    as Ubuntu 18.04, still provide libcurl 7.58.0 in their official
    distribution (libcurl4-openssl-dev). If you use a newer Linux distro
    such as Ubuntu 20.04, you should be fine as the distributed
    libcurl’s version will be above 7.65.0. Another alternative is to
    use plain authentication instead of OAuth2.0.

## License

This package is licensed under the terms of the GPL-3 License.

## References

Crispin, M., *INTERNET MESSAGE ACCESS PROTOCOL - VERSION 4rev1*, RFC
3501, DOI: 10.17487/RFC3501, March 2003,
<https://tools.ietf.org/html/rfc3501>.

Ooms, J. curl: *A Modern and Flexible Web Client for R*. R package
version 3.3, 2019, <https://CRAN.R-project.org/package=curl>

Stenberg, D. *Libcurl - The Multiprotocol File Transfer Library*,
<https://curl.haxx.se/libcurl/>
