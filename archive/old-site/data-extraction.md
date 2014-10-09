New data must be downloaded, extracted and sorted into their corresponding data directories on the local server. Each stream of data (broadly 1.5T data from Toronto General Hospital, and 3T data from the Center for Addiction and Mental Health), requires unique treatment as they are typically collected with differing scanners, protocols and type/number of sequences collected.

The following describes how to use the ready made extraction, transfer and correction scripts to make newly collected data ready to be incorporated into the various pipelines in the lab ( i.e. CIVET, TBSS, MAGeT).

Data Stream 1.5T

All 1.5T are currently collected at Toronto General Hospital and uploaded to a local RAID 4 server. These data must be identified via the SOP, and transferred from the RAID server to the local server. Make note of the Scan ID, used to uniquely identify the subject within the study.

Manual Extraction Scripts

Once the data are in a local folder with your permissions you can copy the TGH specific extraction script to the scan directory:

cp /projects/utilities/Extraction/TGH_EXTRACT.sh <Scan Directory>/

Then enter into the scan directory and call the extraction script, with the first argument as the unique scan ID.

cd <Scan Directory>

./TGH_EXTRACT.sh <Scan ID>

This will initiate scan extraction and conversion of all individual scans into those usable by our current pipelines (i.e. CIVET, Distortion Correction). The result will be a new directory labelled with the Scan ID, that contains all of the pertinent data.

The pertinant scans can be selectively transferred to /data/DT_sample/<Controls/Schizophrenics> using the script "TRANSFER_TGH.sh". However because /data is mounted as read only on each machine, the transfer can only be accomplished on kimsrv, as a user with root permissions.

 

Datastream STOPPD

Due to the multiple sites from which STOPD PD data are collected, slightly different extraciton scripts have been created to logically separate and corrrectly extract the relevant scans for Kimel Lab downstream processing.

Data are transferred through the da124 FTP server, on which the "kimel" user has read and write privelges for each of the site specific directories.

To obtain a scan from the FTP server you can user "secure copy":

scp -r kimel@da124.pet.utoronto.ca:/srv/home/kimel/<SITE>/<Scan Directory>.

It is good idea to preview the directory names either by logging in via ssh, or FTP.

Once the scan has been trasnferred to our local server, site-specific extraction scripts can be found at /projects/david/SCANS/STOP_PD_II/SCRIPTS/<site>_STOP.sh

For example: CAMH_STOP.sh

To extract a STOPPD scan, copy the appropriate extraction script to the subject directory, and execute the script with the first argument as the subject ID.

./CAMH_STOP.sh 24S011

** Manditory QC ***

Three key STOP PD scans must undergo immediate QC after extraciton: 1) Check T1 for artifacts, 2) Check DTI for artifacts and appropriate number of volumes with respect to the diffusion encoding directions, and 3) Review the Resting State scan to confirm that there are 210 timepoints.

*****************

Because the entire protocol is currently taken as a whole, the entire extracted directory can be copied directly to /data/STOP_PD_II/<Site> after QC has been completed.