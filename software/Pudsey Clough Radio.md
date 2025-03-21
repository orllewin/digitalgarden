The technical aspects of [Pudsey Clough Radio](https://orllewin.uk/pcr/)

## Overview

Pudsey Clough Radio runs on a Raspberry Pi Pi400 (see footnote) in my external workshop here in Pudsey Clough. 

The Pi400 runs [Music Player Daemon](https://www.musicpd.org/) to manage the playback of Mp3 files held in the default `mpd` location: `/var/lib/mpd/music` these files are in turn managed by simple .m3u playlists in `/var/lib/mpd/playlists`. 

`mpd` itself is managed by clients and comes with its own basic client `mpc`. With `mpc` you can load different playlists. What `mpd` doesn't have is the ability to schedule playlists so I needed to write something myself to manage that.

The audio stream from `mpd` is sent to [Icecast](https://icecast.org/) and the resulting public stream is proxied by a netradio provider, [Streamerr](https://streamerr.co/) - this means in the unlikely even of more than a handful of listeners things should stay stable, it only costs £3.99 a month.

## scheduler.sh

In order to be able to schedule themed shows I need to be able to load in a new playlist at a specified time, then when the show is complete switch back to a randomised pool of Mp3s, `cron` is the obvious solution with a script that loads a playlist using `mpc`.

Scheduler first validates the playlist argument to check it exists, it currently only schedules a show to play at a specific time daily (fixing that just needs to make use of `$3` in the `cron` line):
```
#!/bin/bash

# Usage: 
# scheduler.sh playlist time day where time is in 24hr format and day is a cron value
# eg. scheduler.sh playlist_name 16:00 *

echo --------------------------------
echo
echo Pudsey Clough Radio scheduler
echo

playlist="$1"
echo "Looking for: $playlist"
playlists=($(mpc lsplaylists))
foundMatch=false

for i in "${playlists[@]}"
do
   echo "  > $i"
   if [ "$i" = "$playlist" ]; then
     foundMatch=true
   fi
done

if [ "$foundMatch" = true ] ; then
  echo
  echo Found $playlist
  echo 
else
  echo
  echo Playlist $playlist is not available
  echo
  exit 1
fi
```

After validating the playlist exists the script parses the input time and creates the `cron` entry pointing to a `play_playlist.sh` script (see below):
```
echo "Start time: $2"

startTime=$2

#This splits the time argument in the format HH:mm:
startHours="${startTime%:*}"
startMinutes="${startTime#*:}"

echo Start hours: $startHours
echo Start minute: $startMinutes

playlistPath="/var/lib/mpd/playlists/$1.m3u"
echo Playlist path: $playlistPath

cronStart="$startMinutes $startHours * * * /home/user/pcr/play_playlist.sh $1"

echo "Cron: $cronStart"

# Write cron entry:
(crontab -u $(whoami) -l; echo "$cronStart" ) | crontab -u $(whoami) -
```

That should take care of starting a show but after completion we need to switch back to random Mp3 playback from the general pool (a generic playlist with no theme), includes `ffprobe` logic found in a Stack Overflow post:
```
playlistSeconds=0.0

while read line; do

trackLength=$(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "/var/lib/mpd/music/$line")

echo "$line: $trackLength"

playlistSeconds=$(dc <<<"$playlistSeconds $trackLength + p")

done <$playlistPath

echo "total playlist seconds (float): $playlistSeconds"

playlistSecondsInt=${playlistSeconds%.*}
echo "total playlist seconds (int): $playlistSecondsInt"

playlistHours=$((playlistSecondsInt/3600))
playlistMinutes=$((playlistSecondsInt%3600/60))
playlistSeconds=$((playlistSecondsInt%60))

echo playlist hours: $playlistHours
echo playlist minutes: $playlistMinutes
echo playlist seconds: $playlistSeconds
```

We now have the hours, minutes, and seconds to add onto the start time to create second `cron` entry which calls another script: `play_general.sh` (that part of the script needs more work so isn't documented here yet)

## play_playlist.sh

A relatively simple bash script to validate the playlist argument then queue it to play. This _should_ use `mpc clear` instead of `mpc crop` but I found that killed the stream (`mpc clear` removes all queued tracks while `mpc crop` removes everything _except_ the currently playing track), this means if a 20 minute track from the random pool had just started playing the scheduled show would start 20 minutes late. Some investigation needed. It also turns off random and repeat which are used when playing from the general pool. 
```
#!/bin/bash

echo 
echo Playlist player
echo 

playlist=$1
echo "Looking for: $playlist"

playlists=($(mpc lsplaylists))
foundMatch=false

for i in "${playlists[@]}"
do
   echo "  > $i"
   if [ "$i" = "$playlist" ]; then
     foundMatch=true
   fi
done

if [ "$foundMatch" = true ] ; then
  echo
  echo Playing $playlist
  echo 

  mpc random off
  mpc repeat off
  mpc crop
  mpc load $playlist
else
  echo
  echo Playlist $playlist is not available
  echo
fi
```

## play_general.sh

A simplified version of `play_playlist.sh` that loads the default playlist back in and reenables random and repeat:

```
#!/bin/bash

echo 
echo Pudsey Clough Radio play general track pool
echo 

mpc crop
mpc load playlist_all
mpc random on
mpc repeat on

echo
echo
```

<hr>

#### Footnote

Lots of people, myself included, do not endorse Raspberry Pi after they hired a former surveillance police offer and their reaction to the backlash ([more details here](https://www.buzzfeednews.com/article/chrisstokelwalker/raspberry-pi-hired-ex-cop-mastodon-controversy)) - they never apologised, instead hoping everyone would forget and move on. I'd bought the Pi400 before this happened and it would be a waste (as in e-waste) not to make good use of it and instead buy new hardware. All the software for Pudsey Clough Radio uses standard Linux tooling and will run on any cheap Linux box.