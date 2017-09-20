---
layout: article
title: "Notes on resting state fMRI preprocessing"
categories: writing
excerpt: "Reading list of good papers on how to treat your data properly."
ads: false
share: false
toc: true
image:
  feature: decor.bldg.jpg
  teaser: cover.fmri.jpg
---

Resting state researchers tend to fall into two camps. One used tissue-based regressors of no interest to remove the influence of non-BOLD noise sources in their data. I like this approach because it is transparent, even if it isn't perfect. The other camp uses Independent Components Analysis to pull out latent variables that represent 'noise' in the data (the topic of brain noise is extremely contentious and hard to parse, because we really don't know what the signal looks like), and these components are regressed out. I don't like this approach because ICA only really works if you know the number of signals generating your data. In the brain that is impossible, and I doubt will ever be possible.

Reading list of good papers on how to treat your data properly. All of these methods can be implemented in `epitome`.

+ [Evaluating the reliability of different preprocessing steps to estimate graph theoretical measures in resting state fMRI data](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4333797/)
+ [Intrinsic Functional Connectivity As a Tool For Human Connectomics: Theory, Properties, and Optimization](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2807224/)
+ [Advances and Pitfalls in the Analysis and Interpretation of Resting-State FMRI Data](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2854531/)
+ [A Component Based Noise Correction Method (CompCor) for BOLD and Perfusion Based fMRI](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2214855/)
+ [Mapping sources of correlation in resting state FMRI, with artifact detection and removal](http://www.ncbi.nlm.nih.gov/pubmed/20420926)

