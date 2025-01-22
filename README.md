# auto_fs_os_x
autofs on OSX (year: 2025)

## Create autofs 'base directory'

Note: In this example I'll use /System/Volumes/Data/mount as a autofs `base directory`.

    sudo mkdir /System/Volumes/Data/mount

## Setup autofs files
Edit: /etc/auto_master: (sudo vi /etc/auto_master)

    #
    # Automounter master map
    #
    +auto_master		# Use directory service
    #/net			-hosts		-nobrowse,hidefromfinder,nosuid
    /home			auto_home	-nobrowse,hidefromfinder
    /System/Volumes/Data/mount	auto_nfs
    /Network/Servers	-fstab
    /-			-static

Edit/Create: /etc/auto_nfs: (sudo vi /etc/auto_nfs)

    movies  -fstype=nfs,nfsvers=4,soft,bg,intr,rw,tcp,resvport 172.16.100.8:/volume1/shared-media/Movies
    tvshows	-fstype=nfs,nfsvers=4,soft,bg,intr,rw,tcp,resvport 172.16.100.8:/volume1/shared-media/TVShows
## Restart autofs

Restart automounter (unless you already rebooted!)

    sudo automount -cv

Your mounts should be available in `/System/Volumes/Data/mount/movies` and `/System/Volumes/Data/mount/tvshows`

## Create symbolic link from /... to ...

I want my shares to be available via `/libraries/movies` and `/libraries/tvshows`. Via synthetic you can create links in `/`

**NOTE**: there *MUST* be a TAB between `libraries` and `System/Volumes/Data/mount`. The internet tells stories about stuck installations if you use spaces, be warned!

Edit/Create: /etc/synthetic.conf: (sudo vi /etc/auto_synthetic.conf)

    libraries  System/Volumes/Data/mount

(Note: a reboot might be required for synthetic to function)
