# plex

Mkay, first (cut a hole in the box? nah...), this runs on my ESXi box, so the starting point here is my PXE CentOS 7 minimal install.

1. Install docker
2. `yum install cifs-utils`
3. mkdir /mnt/{media1, media2, ssmedia}
4. Mount media drives to the ESXi VM
     - `mount -t cifs -o username=quasarname,password=quasarpass //quasar/media1 /mnt/media1`
     - `mount -t cifs -o username=quasarname,password=quasarpass //quasar/media2 /mnt/media2`
     - `mount -t cifs -o username=windosname,password=windowspass //x-corp/ssmedia /mnt/ssmedia`
5. run docker container with these volumes:
     - `-v /mnt/media1/Movies:/data/movies`
     - `-v /mnt/media2/Movies:/data/movies`
     - `-v /mnt/ssmedia/Movies:/data/movies`
     - `-v /mnt/ssmedia/TV:/data/tv`
     - `-v /media/plexlibrary:/config`

Example of docker run command:
```
docker run --name=plex --net=host -e VERSION=latest -v /mnt/media1/Movies:/data/movies1 -v /mnt/media2/Movies:/data/movies2 -v /mnt/ssmedia/Movies:/data/movies3 -v /mnt/ssmedia/TV:/data/tv -v /media/plexlibrary:/config linuxserver/plex
```

Using the [`linuxserver/plex`](https://hub.docker.com/r/linuxserver/plex/) docker image.
