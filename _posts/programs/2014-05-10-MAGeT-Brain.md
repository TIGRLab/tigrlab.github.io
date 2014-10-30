---
layout: media
title: "MAGeT Brain"
categories: programs
excerpt: "Multiple Automatically Generated Templates Brain segmentation algorithm."
ads: false
share: false
image:
  feature: projects.dti-aging.large.jpg
  teaser: projects.dti-aging.small.jpg
---

Given a set of labelled MR images (atlases) and unlabelled images (subjects), MAGeT produces a segmentation for each subject using a multi-atlas voting procedure based on a template library made up of images from the subject set.

The major difference is that, in MAGeT brain, segmentations from each atlas (typically manually delineated) are propogated via image registration to a subset of the subject images (known as the 'template library') before being propogated to each subject image and fused. It is our hypothesis that by propogating labels to a template library, we are able to make use of the neuroanatomical variability of the subjects in order to 'fine tune' each individual subject's segmentation.

[See the CoBrA lab website for more](http://cobralab.ca)
[See the CoBrA lab GitHub for software](https://github.com/CobraLab/MAGeTbrain)

To cite MAGeT brain in publications, please use:

> Pipitone J, Park MT, Winterburn J, et al. Multi-atlas segmentation of the whole hippocampus and subfields using multiple automatically generated templates. Neuroimage. 2014;