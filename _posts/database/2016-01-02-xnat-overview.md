---
layout: media
title: "Overview"
excerpt: "An introduction to our MRI database."
categories: database
ads: false
share: false
toc: true
image:
  feature: banner.database.jpg
  teaser: teaser.database.jpg
---

Home Page
---------

The home page of XNAT looks like this:

<figure>
	<a href="/images/guide.xnat-home.jpg"><img src="/images/guide.xnat-home.jpg"></a>
</figure>

On the left is a list of **projects**. Each project contains a set of scans for a particular study. In studies where we are collecting data from multiple sites, the project name ends with a number (`01`, `03`, etc). You will only be given access to the projects that you are directly involved in, so this list might be very short.

<figure>
	<a href="/images/guide.xnat-home-projects.jpg"><img src="/images/guide.xnat-home-projects.jpg"></a>
</figure>

On the right is a list subjects with recent changes. For example, you should see your new data uploads at the top of this list.

You can **change your password** from the home page by clicking on your user name (hidden at the top):

<figure>
	<a href="/images/guide.xnat-home-user.jpg"><img src="/images/guide.xnat-home-user.jpg"></a>
</figure>


Data Organization
-----------------

XNAT has a simple data organization format:

+ Subject
    + Session
        + Series

Each subject represents a particular person, each session represents a particular visit to the MRI, and each series represents a particular MRI sequence run on that person.

We have elected not to use the sessions in XNAT, and therefore, **all session and subject names should be identical**. Much more on that issue in our 'naming' guide.

Let's look at the session for subject `SPN01_CMH_PHA_FBN0065`, a fBIRN phantom from the SPINS study, by clicking on it under 'Recent Data Activity':

<figure>
	<a href="/images/guide.xnat-session.jpg"><img src="/images/guide.xnat-session.jpg"></a>
</figure>

The list at the bottom are all of the series contained within the current session.

At the side is a set of commands one can use to do manage the data, such as deleting bad files, renaming the subject and/or session, or downloading copies of the data.

Along the top, you can see that we are actually inside the session `SPN01_CMH_PHA_FBN0065` for subject `SPN01_CMH_PHA_FBN0065`. This is because we ask that all session and subject names match. If you click `SUBJECT: SPN01_CMH_PHA_FBN0065`, you should see this:

<figure>
	<a href="/images/guide.xnat-subject.jpg"><img src="/images/guide.xnat-subject.jpg"></a>
</figure>

Which lists all of the sessions for this subject.

Here, you can also manage the data from the right hand sidebar (rename the subject, or delete the data).

**In our database, we want each subject to have only one session. This is covered in more detail in our naming guide. Therefore, this screen should only ever have one session in it.**
