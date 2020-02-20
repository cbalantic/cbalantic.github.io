---
title: "Our R package is out! AMMonitor: Remote Biodiversity Monitoring in an Adaptive Framework"
layout: post
---

**AMMonitor** -- the R software package my collaborators and I been working on for the past several years -- is finally public, and available for free under provisional release from the U.S. Geological Survey Gitlab repository here: [https://code.usgs.gov/vtcfwru/ammonitor](https://code.usgs.gov/vtcfwru/ammonitor). A brief software paper documenting this work is currently in review.

![](http://cbalantic.github.io/images/ammonitor-footer.png)

The package is extensively documented on the Wiki associated with the Gitlab site: [https://code.usgs.gov/vtcfwru/ammonitor/wikis/home](https://code.usgs.gov/vtcfwru/ammonitor/wikis/home). On the right-hand panel of that link, you'll find 20 chapters of documentation, meant to be worked through in sequence. There are sample datasets and examples to illustrate functionality and workflows. **AMMonitor** is geared specifically toward remote automated monitoring situations, and many of our workflows have been heavily influenced by [our use of smartphones as remote automated monitoring units (AMUs)](https://cbalantic.github.io/temporally-adaptive-published/) -- though you don't need to use smartphones for monitoring to use this package. Even if you never need to implement this type of monitoring or use this R package in your work, I think the general vision of the package offers a lot to the imagination. 

**AMMonitor** contains functions and workflows that support application of the methodology we've written about in my PhD publications, such as [dynamic wildlife occupancy models using automated acoustic monitoring data](https://esajournals.onlinelibrary.wiley.com/doi/abs/10.1002/eap.1854),  [temporally adaptive acoustic sampling](https://onlinelibrary.wiley.com/doi/full/10.1002/ece3.5579), and [statistical learning mitigation of false positives in automated acoustic wildlife monitoring](https://www.tandfonline.com/doi/full/10.1080/09524622.2019.1605309).

However, the general vision of the package moves beyond that. More broadly, the package aims to support automated or semi-automated ecological data collection workflows such that progress toward management and research objectives can be systematically tracked through time. This vision is heavily influenced by the idea of Adaptive Management (whence comes the "AM" in AMMonitor), wherein uncertainty about a system is reduced through time via systematic monitoring. One of the best examples of ecology-related Adaptive Management that I'm aware of is the [migratory waterfowl adaptive harvest management program](https://www.fws.gov/birds/management/adaptive-harvest-management.php) implemented by the U.S. Fish and Wildlife Service ([Nichols et al. 1995](https://digitalcommons.unl.edu/usfwspubs/373/) and [Johnson et al. 2015](https://wildlife.onlinelibrary.wiley.com/doi/full/10.1002/wsb.518) are some literature go-tos on this subject). One of the primary motivations behind **AMMonitor** was to be able to offer a framework for streamlining adaptive management that uses automated data collection.

Please check the package out if of interest, and get in touch with questions!

