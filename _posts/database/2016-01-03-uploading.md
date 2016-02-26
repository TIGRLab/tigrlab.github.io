---
layout: media
title: "Uploading"
excerpt: "How to get your DICOMs to us."
categories: database
ads: false
share: false
toc: true
image:
  feature: banner.database.jpg
  teaser: teaser.database.jpg
---

Upload Overview
---------------

+ Ensure your raw DICOM scans for a given participant are in each in their own folder.
+ **Do not** include NIFTI or otherwise converted files in this package.
+ **Do not** include any pre-processed data from the scanner (e.g., motion corrected data, FA maps, etc.) These pollute the database and we can generate better images in post-processing.
+ Package your raw DICOM folders into a appropriately named session ZIP file (e.g., PRE01_MRC_032_01_01).
+ Upload your ZIP file to the database using the browser-based GUI.

Create New Subjects
-------------------

Subject naming follows the basic convention:

    [STUDY]_[SITE]_[SUBJECT]_[TIMEPOINT]_[REPEAT]

+ Study IDs will be consistent across all sites (PRE01), even though they are uploaded to different projects in XNAT (e.g., PRE07 for site ZHH).
+ Site IDs are assigned by us -- a lookup table in included on this page.
+ Subject IDs must be assigned by your study coordinator, can be any length, and generally should be the same as those used in the clinical database (if applicable).
+ Timepoints are alpha-numeric codes denoting the timepoint in a multi-scan study (e.g., for baseline-followup, 01 & 02, or BL & FU).
+ Repeats are numeric codes denoting the rare case that a participant needs to be scanned more than once for a particular timepoint.

To create a subject manually (most of the time, this can be done automatically during the upload), first enter a project, then from the top bar:

+ Click 'new' --> 'subject'
+ Enter the subject ID, DOB, gender, & handedness (if required).

Upload DICOM Images as .zip
---------------------------

Frop top of Subject page:

+ Click 'upload' --> 'images'
+ At bottom of this page, click 'click here' for the alternative upload method.
+ Choose a DICOM zip file, then 'Begin Upload' (and wait for upload -- this can take a while).
+ Click 'Upload' --> 'Go to prearchive'.
+ Select uploaded session, then click 'Archive'.
+ Check/correct subject ID, make session ID identical to subject ID, & review other fields. These should be automatically set by the DICOM headers, so if there are consistent errors here, you should be able to correct them by speaking with your MR team and ensuring the data is entered correctly as the scanner.
+ Click 'Submit'.

The precise naming of the DICOM header fields will automate most of the session and scan generation process, which should make your life rather easy.

Uploading Non-DICOM Data
------------------------

If collected, extra datatypes (e.g., physiological or behavioural) data collected during the MRI scans need to be uploaded separately. These must be added to a subject's session, which is most easily done after the DICOM files have been uploaded. As a best practice, these .zip files should be names the full subject name with an _FILETYPE.zip (e.g., PRE01_MRC_0023_01_01_PHYSIO.zip). This is more to ensure the data is organized correctly before upload.

Additionally, non-image uploads could also be technologist notes or QC .pdf files. These should be kept separate from the other data uploads.

XNAT keeps the data organized under each subject's session into 'resources/' and 'scans/'. The scans/ folder holds all of the uploaded DICOM data, and the resources/ folder contains everything else. Therefore, under resources, create the following folders are nessicary:

+ resources/logs -- participant log sheets (.xls, .csv, .pdf, etc.)
+ resources/behav -- behavioural data for fMRI experiments.
+ resources/physio -- physiological recordings collected during the MRI protocol.

More details follow:

+ From the subject page, click on the 'mr session'.
+ Click 'Manage Files'. You should see a list of the DICOM scans under scans/ and a resources/ folder (potentially with some contents).
+ To create a physio folder: Click 'Add Folder'. Set 'Level' to 'resources' and 'folder' to 'physio'. Click create.
+ To create a behav folder: Click 'Add Folder'. Set 'Level' to 'resources' and 'folder' to 'behav'. Click create. Et cetera.
+ To upload physio data to this newly created folder: Click 'Upload Files', 'Level' == 'resources', 'Folder' == 'physio'. Click 'Browse' and attach the 'SUBJNAME_PHYSIO.zip file. Click 'Upload'.
+ Click 'extract files to server'.
+ The above works for all filetypes (e.g., a single .pdf), but .zip is the preferred way of uploading multiple files at once.
+ Ensure the uploaded data is all present in the tree.
