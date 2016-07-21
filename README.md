# plex

Mkay, first (cut a hole in the box? nah...), this runs on my ESXi box, so the starting point here is my PXE CentOS 7 minimal install. I'm running Plex inside of a docker container, but the ESXi VM needs some additional configuration first. I'm using `autofs` to keep my media drives mounted. It will mount the drives when they're needed (and present) and keep them around for 30 minutes. I also set up a cron job to `ls` into each directory every 14 minutes, to ensure that they stick around.

The plex configuration directory is `/opt/plexlibrary`.
**VITAL NOTE!!: I still need to figure out a plan for backing up this directory!!**

Then run the docker command below with all the relevant directories and we're off to the races.
**I think restarting the docker container updates plex, need to verify**


1. Install docker
2. `yum install cifs-utils autofs samba-client samba-common`
3. mkdir /automounts
4. mkdir /opt/plexlibrary
5. `sudo vi /etc/auto.master`      # add the following line
     - `/automounts      /etc/auto.automounts     --timeout=1800  --ghost`
6. add `auto.automounts` file to `/etc/`
7. enable and start autofs
     - `systemctl enable autofs`
     - `systemctl start autofs`
     - `systemctl status autofs`
8. test each directory in `/automounts`
9. add `ls` cron job to keep directories mounted
     - `crontab -e`
          - `*/14 * * * * /bin/ls /automounts/qsrmovs1/Movies >/dev/null 2>&1`
          - `*/14 * * * * /bin/ls /automounts/qsrmovs2/Movies >/dev/null 2>&1`
          - `*/14 * * * * /bin/ls /automounts/xcmovies >/dev/null 2>&1`
          - `*/14 * * * * /bin/ls /automounts/xctv >/dev/null 2>&1`
10. `sudo mkdir /opt/plexlibrary`
11. run docker container with these volumes:
**NOTE**: the automounted directories must be mounted when docker starts, so navigate to each of them before running the docker command below!!!
     - `-v /automounts/qsrmovs1/Movies:/data/movies1`
     - `-v /automounts/qsrmovs2/Movies:/data/movies2`
     - `-v /automounts/xcmovies:/data/movies3`
     - `-v /automounts/xctv:/data/tv1`
     - `-v /opt/plexlibrary:/config`

Example of docker run command:
```
docker run --name=plex --net=host -e VERSION=latest -v /automounts/qsrmovs1/Movies:/data/movies1 -v /automounts/qsrmovs2/Movies:/data/movies2 -v /automounts/xcmovies:/data/movies3 -v /automounts/xctv:/data/tv1 -v /opt/plexlibrary:/config linuxserver/plex
```

Using the [`linuxserver/plex`](https://hub.docker.com/r/linuxserver/plex/) docker image.
