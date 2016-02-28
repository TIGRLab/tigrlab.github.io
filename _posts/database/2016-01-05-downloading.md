---
layout: media
title: "Downloading"
excerpt: "How to download DICOMs stored with us."
categories: database
ads: false
share: false
toc: true
image:
  feature: banner.database.jpg
  teaser: teaser.database.jpg
---

Let's say we want to download the T1 weighted image, T2 weighted image, and FLAIR image, from a subject in the PACTMD study. This subject was scanned on the CAMH scanner. We're going to download the data from the first session of subject 0110004.

From the home page, we select the PACTMD study, the subject `PACTMD_CMH_0110004_01_01`, and then the first (and only) session for that subject:

<figure>
	<a href="/images/guide.xnat-go-to-subject.gif"><img src="/images/guide.xnat-go-to-subject.gif"></a>
</figure>

On the right hand bar, we select `download`, and then `download images`:

<figure>
	<a href="/images/guide.xnat-dl-link.gif"><img src="/images/guide.xnat-dl-link.gif"></a>
</figure>

Here we select our output file type (`.zip`), the series we want (T1, T2, and FLAIR), and finally click download:

<figure>
	<a href="/images/guide.xnat-dl-dicoms.gif"><img src="/images/guide.xnat-dl-dicoms.gif"></a>
</figure>

The DICOM files selected will be in the archive saved to your computer. You might need a program to 'unzip' your data such as [WinZip](http://www.winzip.com) for Windows or [UnRarX](http://unrarx.en.softonic.com/mac) for Mac.
