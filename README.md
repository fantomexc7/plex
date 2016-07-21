# plex

Mkay, first (cut a hole in the box? nah...), this runs on my ESXi box, so the starting point here is my PXE CentOS 7 minimal install.

1. Install docker
2. `yum install cifs-utils autofs samba-client samba-common`
3. mkdir /automounts
4. mkdir /opt/plexlibrary
5. `sudo vi /etc/auto.master`      # add the following line
     - `/automounts      /etc/auto.automounts     --timeout=600  --ghost`
6. add `auto.automounts` file to `/etc/`
7. enable and start autofs
     - `systemctl enable autofs`
     - `systemctl start autofs`
     - `systemctl status autofs`
8. test each directory in `/automounts`
9. `sudo mkdir /opt/plexlibrary`
10. run docker container with these volumes:
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
