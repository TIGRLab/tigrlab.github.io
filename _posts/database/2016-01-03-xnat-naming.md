---
layout: media
title: "File naming"
excerpt: "Why PRE01_ZHH_003020_01_02 is so important."
categories: database
ads: false
share: false
toc: true
image:
  feature: banner.database.jpg
  teaser: teaser.database.jpg
---

DICOM Header Requirements
-------------------------

The XNAT database requires some information in the DICOM header to be intact.

+ (0008,0020), (0008,0021), (0008,0022) -- Scan Date.
+ (0010,0010) -- Patient Name (e.g., PRE01_MRC_021_01_01).
+ (0010,0020) -- Patient ID (e.g., PRE01_MRC_021_01_01).

Now, the data will be included alongside the DICOM images in future downloads.

List of Codes
-------------

**Sites**

|Site|Full Name                              |Assigned Projects |
|----|---------------------------------------|------------------|
|IWA |U Iowa College of Medicine             |PRE02             |
|MIN |U Minnesota                            |PRE03             |
|GRD |Grady Hospital                         |PRE04             |
|CRY |Cherry Street                          |PRE05             |
|BRG |Borgess WMU School of Medicine         |PRE06             |
|MAS |U Massachusetts                        |PRE07             |
|CTN |Creighton U                            |PRE08             |
|PCE |Peace Health                           |PRE09             |
|SFD |Stanford U                             |PRE10             |
|TXS |U Texas                                |PRE11             |
|MCH |U Michigan                             |PRE12             |
|MRC |Maryland Psychiatric Research Center   |SPN03             |
|ZHH |Zucker Hillside Hospital               |SPN02             |
|PMC |Pittsburg Medical Center               |STOPPD04          |
|CMH |Centre for Addiction and Mental Health |Many              |
|TGH |Toronto General Hospital               |DTI01             |
|MAS |University of Massachusetts            |STOPPD02          |
|NKI |Nathan Kline Institute                 |STOPPD03          |

**Phantoms**

|Name|Full Name                                 |Studies       |
|----|------------------------------------------|--------------|
|ADN | ADNI Magphan phantom                     | PREXX, SPNXX |
|FBN | fBIRN phantom                            | SPNXX        |
|ACR | American college of radiologists phantom | PREXX        |

Naming Convention Specifics
---------------------------

**Participants**

+ SPN01_CMH_0009_01_01
+ [STUDY]\_[SITE]\_[SUBJECT]\_[SESSION]\_[REPEAT]
+ In XNAT, there should be exactly one session per subject. To enter a second session, create a new subject with the appropriate name (e.g., SESSION = 02).

**Human Phantoms / Repeats**

In some studies, special subjects are being acquired (for example, traveling human phantoms to test scanner reliability between sites, or repeat scans to assess signal variability within sites). We can mark these by inserting a letter code in front of the subject code:

+ P -- human phantom
+ R -- repeat scan

E.g. SPN01_CMH_P009_01_01 or PRE04_ZHH_R3562_01_01.

**Non-Human Phantoms**

+ SPN01_CMH_PHA_ADN0001
+ [STUDY]\_[SITE]\_PHA\_[PHANTOM]\_[SESSION]
+ Phantom scans should be run regularly, and their session names will be numeric ascending.
+ Our web-based data viewing system requires the date fields in the DICOM header be intact.


As an example of how the data is organized, see the below:

+ Subject 1 (SPN01\_CMH\_0001\_01\_01)
    + Session 1 (SPN01\_CMH\_0001\_01\_01)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI)

Now see what we would do if a person needed to come back to have a repeat scan done due to a bad acquisition.

+ Subject 2 (SPN01\_CMH\_0002\_01\_01)
    + Session 1 (SPN01\_CMH\_0001\_01\_01)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI -- bad, needs to be redone)
+ Subject 2 (SPN01\_CMH\_0002\_01\_02)
    + Session 2 (SPN01\_CMH\_0001\_01\_02)
        + Series 1 (Localizer)
        + Series 3 (DTI)
