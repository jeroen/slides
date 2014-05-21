---
title       : Embedded Scientific Computing
subtitle    : A reliable, scalable and reproducible approach to statistical software for data-driven business and open science
author      : Jeroen Ooms
job         : UCLA Statistics
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
---

<!-- 
library(slidify)
library(slidifyLibraries)
-->

## The changing role of Statistical Software

<!-- 
- fundamental change in the way analysis and vizualization methods get implemented 
- central: observation that field is rapidly changing
-->

![gears](gears.jpg)

### Background and Motivation

> - What and why
> - Current Developments
> - Implications for Statisticians

### The Research

> - Identify Problems
> - Experiment with Approaches
> - Suggestions and Recommendations

---

## Current Developments

![datasharing](datasharing.jpg)

### Internet
 > - Scientific collaboration
 > - Reproducibility, transparency, eduction
 > - Data management and scaling
 > - Browser-based graphics, UI
 
### Growth of the field
 > - Competition
 > - Specialization of software

---


## Interoperability

<!-- 
- Statistics and statisticians becoming more accessible is accompanied by interoperability of statistical software.
-->

 > - Integrate statistical methods in upcoming technology for big data, vizualization
 > - Develop specialized applications
 > - Real-time analysis on live data in pipelines
 > - Internet based data management and sharing
 > - Online analysis and scientific collaboration
 > - Transparency and accountability by reproducibility
 > - Learning and teaching 

---

## Embedded Scientific Computing

### Purpose

Integrating statistical methods in third party software in a way that is:

> - Reliable
> - Scalable
> - Practical

### Research

> - Identify problems
> - Experiment with solutions
> - Suggestions and recommendations

---

## The Four Cornerstones

![puzzle](puzzle.jpg)

### Core problems:

> - Interfacing statistical methods
> - Security and resource control
> - Data interchange
> - Version management

### For each topic:

> - What?
> - Why is it a fundamental problem?
> - Domain specific aspects
> - My solution

---

## The Four Cornerstones

<!-- 
First discuss implementation problems
Then move towards more high level design choices
-->

![puzzle](puzzle.jpg)

### Core problems:

- <u><b>Interfacing statistical methods</b></u>
- <u><b>Security and resource control</b></u>
- Data interchange
- Version management

### For each topic:

- What?
- Why is it a fundamental problem?
- Domain specific aspects
- My solution

---

## Security and resource control

> - Most challenging piece
> - Problem: controlling
> - Traditional user-role based 

### Domain specific problems:

> - Use role based security infeasible
> - Need for arbitrary code execution
> - Control hardware resources

---

## Example security profile

```no-highlight
profile r-base {
    #include <abstractions/base>
    #include <abstractions/nameservice>
    @{PROC}/[0-9]*/attr/current r,    

    /bin/* rix,
    /etc/R/* r,
    /etc/fonts/** mr,
    /etc/resolv.conf r,
    /tmp/** rw,
    /usr/bin/* rix,
    /usr/lib/R/bin/* rix,
    /usr/lib{,32,64}/** mr,
    /usr/lib{,32,64}/R/bin/exec/R rix,
    /usr/local/lib/R/** mr,
    /usr/local/share/** mr,
}
```

---

## Mandatory Access Control

RAppArmor: bindings security methods in `Linux`:


```r
#Set 100M memory limit
> rlimit_as(100 * 1024 * 1024)

#Set 4 core limit
> rlimit_nproc(4)

#apply security profile
> aa_change_profile("my_secure_profile")

#not allowed
> list.files("/")
character(0)
```


---

## Dynamic Sanboxing with eval.secure


```r
#Sandboxed evaluation
> eval.secure({
  #arbitrary code
  x <- rnorm(3)
  mean(x)
#With restrictions  
}, profile="my_secure_profile", rlimit_as = 100 * 1024 * 1024, rlimit_nproc = 4)
[1] 0.01563452
```


Dynamic sandboxing with `eval.secure`:

> 1. Create fork of the current process
> 2. Apply limits and security profile
> 3. Evaluate code in fork
> 4. Copy output to parent proc
> 5. Kill fork (and children)

---

## Mandatory Access Control

### Major benefits for scientific computing

> - No need for complex user-role security policies
> - Separate security concern from computing concerns
> - Support for arbitrary code execution
> - Fine grained control over hardware allocation
> - Scale up to many users without sacrifing reliability
> - Performance overhead neglectible  

---

## Interfacing

- Interoperable
- Separation of concerns
- HTTP
- State problem
