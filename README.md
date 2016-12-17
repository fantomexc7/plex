# HTPC Project
A virtual machine is going to run multiple docker containers, each container providing a specific function of the media center. Docker containers will include:
* plex
* sabnzbd
* sonarr
* couchpotato

The virtual machine will need to need read/write access to my media warehouse, as well as the downloads directories (and maybe even config directories for each service?). The docker containers can then mount their relevant directories and manage their portion of the whole system. Here's what each service will be responsible for:

* sonarr: tracks the TV shows I want to capture, then finds the .nzb (/maybe torrent files?). Passes .nzb to sabnzbd for downloading
* couchpotato: tracks the movies I want to capture, then finds the .nzb and passes it along to sabnzbd for downloading
* sabnzbd: downloads files passed to it from sonarr/sabnzbd
* plex: media server

#### Remaining questions
* does sabnzbd rename/process anything?
* is plex is completely in charge of library management?
