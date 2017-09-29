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

XNAT Naming Convention
----------------------

As a general rule, we want all of the information about a file (the project, subject, timepoint, and any other details) directly in the filenames of every file, so we can always determine where our data came from at a glance. In detail:

+ **project** : What experiment is this data associated with?
+ **site** : What scanner did this data come from?
+ **subject** : What unique individual is this data associated with?
+ **timepoint** : In multiple time-point studies, we want to know which time point this data is from (e.g., baseline, 6 month follow-up, etc.)
+ **repeat** : Sometimes, data quality is bad, and the participant needs to come back to collect more data for the same time point. Other times, the participant needs to go to the bathroom. Every time a participant renters the scanner for a particular time point, they get a new repeat number.

We also distinguish between human, special human, and non-human phantom data (objects scanned at each site to evaluate scanner performance). They all have different naming conventions.

**Participants**

+ `[STUDY]_[SITE]_[SUBJECT]_[TIMEPOINT]_[REPEAT]`
+ `SPN01_CMH_0009_01_01`
+ Your **site** is unique to your scanner. See the list of site codes below.
+ In XNAT, there should be exactly one session per subject and timepoint. To enter a second timepoint, create a new subject with the appropriate name (e.g., `SPN01_CMH_0009_02_01`).

**Special Participants: Human Phantoms / Test-Retest Repeats**

In some studies, special subjects are being acquired (for example, traveling human phantoms to test scanner reliability between sites, or test-retest repeat scans to assess signal variability within sites). We can mark these by inserting a letter code in front of the subject code:

+ `P` -- human phantom
+ `R` -- test-retest repeat scan

Naming is otherwise the same as for normal participants:

+ `[STUDY]_[SITE]_[SUBJECT]_[TIMEPOINT]_[REPEAT]`
+ `SPN01_CMH_P009_01_01`
+ `PRE04_ZHH_R3562_01_01`.

**Non-Human Phantoms**

+ `[STUDY]_[SITE]_PHA_[PHANTOM]_[TIMEPOINT]`
+ SPN01_CMH_PHA_ADN0001
+ Phantom scans should be run regularly, and their session names will be numeric ascending.
+ Our web-based data viewing system requires the date fields in the DICOM header be intact.

Naming Examples
---------------

As an example of how the data is organized, imagine a (poorly designed) study with two subjects where we collect T1 and DTI data at baseline, prescribe one of them a drug, and then scan them again in one year. Recall that XNAT organizes data into a

+ Subject
    + Session
        + Series

format, but we want all of our subjects and sessions in XNAT to have the same names. The baseline data might look like this for subject one:

+ Subject 1 (`SPN01_CMH_0001_01_01`)
    + Session 1 (`SPN01_CMH_0001_01_01`)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI)

Subject two was scanned, but moved a lot during the DTI, so we can't analyze it. So we brought that person back for a repeat scan due to the bad acquisition, and name it like this:

+ Subject 2 (`SPN01_CMH_0002_01_01`)
    + Session 1 (SPN01_CMH_0002_01_01`)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI -- bad, needs to be redone)
+ Subject 2 (`SPN01_CMH_0002_01_02`)
    + Session 2 (`SPN01_CMH_0002_01_02`)
        + Series 1 (Localizer)
        + Series 2 (DTI)

Finally, the 1 year follow up scans for the subjects would look like this:

+ Subject 1 (`SPN01_CMH_0001_02_01`)
    + Session 1 (`SPN01_CMH_0001_02_01`)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI)
+ Subject 2 (`SPN01_CMH_0002_02_01`)
    + Session 1 (`SPN01_CMH_0002_02_01`)
        + Series 1 (Localizer)
        + Series 2 (T1)
        + Series 3 (DTI)

DICOM Header Requirements
-------------------------

The XNAT database will **automatically name your files** correctly so long as you make suresome information in the DICOM header is correct:

+ `(0008,0020)`, `(0008,0021)`, `(0008,0022)` -- Correct scan date.
+ `(0010,0010)` -- Patient Name (e.g., `PRE01_MRC_021_01_01`).
+ `(0010,0020)` -- Patient ID (e.g., `PRE01_MRC_021_01_01`).

`(0010,0010)` and `(0010,0020)` map onto the subject and session, respectively. During uploads, if these fields are correct, everything will end up in the proper place of the database.

Now, the data will be included alongside the DICOM images in future downloads.

Acronyms
--------

**Sites**

|Site|Full Name                              |Assigned Projects |
|----|---------------------------------------|------------------|
|IWA |U Iowa College of Medicine             |PRE02             |
|MIN |U Minnesota                            |PRE03             |
|GRD |Grady Hospital                         |PRE04             |
|BRG |Borgess WMU School of Medicine         |PRE05             |
|CRY |Cherry Street                          |PRE06             |
|MAS |U Massachusetts                        |PRE07             |
|CTN |Creighton U                            |PRE08             |
|PCE |Peace Health                           |PRE09             |
|SFD |Stanford U                             |PRE10             |
|TXS |U Texas                                |PRE11             |
|MCH |U Michigan                             |PRE12             |
|NWN |Northwestern U                         |PRE14             |
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
