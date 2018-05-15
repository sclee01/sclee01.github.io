---
layout: post
title: Set working directory permanently for R on Mac
excerpt: "R에서 워킹 디렉토리를 영구 설정하는 방법"
date: 2018-05-15
modified: 2018-05-15
author: sclee
categories: blog
tags: [R]
image:
  url: http://i.imgur.com/p0TzIOM.gif
comments: true
share: true
---

## R에서 워킹 디렉토리 영구적으로 설정하기
매번 R studio 혹은 R script을 실행해서 `setwd("디렉토리 경로")`를 설정하는 것은 매우 불편하고 짜증스럽습니다.

Mac에서 간단하게 워킹 디렉토리를 영구적으로 설정하는 방법에 대해 알아봅시다.
보통 초기 디렉토리는 `/Users/login_name` 입니다. 여기에 `.Rprofile` 파일을 생성합니다.R session을 생성할때 `.Rprofile`을 참조하여 열게 되는데 여기다가 initialize할 명령어들을 입력해 두면 됩니다.

아래 소스코드를 복사하여 `.RProfile`에 복사하면 됩니다.
저의 경우 `/Users/sclee/dev/R`에 워킹 디렉토리를 영구적으로 설정하였습니다. 그밖에 다른 라이브러리를 자동으로 불러오는 경우 추가적으로 코드를 넣어주면 됩니다.

```
##                      Emacs please make this -*- R -*-
## empty Rprofile.site for R on Debian
##
## Copyright (C) 2008 Dirk Eddelbuettel and GPL'ed
##
## see help(Startup) for documentation on ~/.Rprofile and Rprofile.site

# ## Example of .Rprofile
# options(width=65, digits=5)
# options(show.signif.stars=FALSE)
# setHook(packageEvent("grDevices", "onLoad"),
#         function(...) grDevices::ps.options(horizontal=FALSE))
# set.seed(1234)
 .First <- function() cat("\n   Welcome to R!\n\n")
setwd("/Users/sclee/dev/R")
 .Last <- function()  cat("\n   Goodbye!\n\n")

# ## Example of Rprofile.site
# local({
#  # add MASS to the default packages, set a CRAN mirror
#  old <- getOption("defaultPackages"); r <- getOption("repos")
#  r["CRAN"] <- "http://my.local.cran"
#  options(defaultPackages = c(old, "MASS"), repos = r)
#})
```

---

참고 문서
1. [https://www.r-bloggers.com/setting-your-working-directory-permanently-in-r/](https://www.r-bloggers.com/setting-your-working-directory-permanently-in-r/)
