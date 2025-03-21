My memory...

### Generate bordered/no-border sample images

Needed for a project, a batch of images, some with a border, some without:

```
#!bin/bash

for i in $(seq 1 10);
do	
	width=$(jot -r 1 100, 400)
	height=$(jot -r 1 100, 400)
	
	if (( $(jot -r 1) > 50 )); then

		borderDiam=$(jot -r 1 1, 20)
		$(magick -size ${width}x${height} xc:gray +noise random -colorspace gray -bordercolor white -border ${borderDiam} bordered${i}.jpg)
	else
		$(magick -size ${width}x${height} xc:gray +noise random -colorspace gray bordered${i}.jpg)
	fi
done
```

Similar to above, but fixed size and draws colours circles to help with testing image scaling:

```
#!bin/bash

path="app/src/main/res/drawable-nodpi/sample"
for i in $(seq 1 16);
do	
	width=256
	height=256
	
	$(rm -f ${path}${i}.png)
	if (( $(jot -r 1) > 25 )); then
		borderDiam=$(jot -r 1 1, 40)
		$(magick -size ${width}x${height} xc:gray +noise random -colorspace gray -fill blue -draw "circle 128,128 165,128" -bordercolor white -border ${borderDiam} ${path}${i}.png)
	else
		$(magick -size ${width}x${height} xc:gray +noise random -colorspace gray -fill red -draw "circle 128,128 165,128" ${path}${i}.png)
	fi
done
```
### Remove Git

I always forget this:

```
rm -rf .git
```

## Quick Git Push

Obviously not for projects that need a readable git history, but for blogs and similar:

```sh
#!/bin/bash

timestamp=$(date +%s)
echo "Saving: $timestamp"
git add .
git commit -m "$timestamp"
git push origin main
```

Save as `p.sh` and make executable: `chmod a+x p.sh` then run with `./p.sh`

## Repoclean
Hack to delete the commit history of a repo: `sh repoclean.sh`
```bash
#!/bin/bash

git checkout --orphan tmp
git add -A
git commit -m '...'
git branch -D main
git branch -m main
git push -f origin main
```

## Banish .DS_Store
```
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch
echo .DS_Store >> .gitignore
git add .gitignore
git commit -m "DS_Store begone"
```
## Find String in Files
Will scan recursively in current directory
```sh
grep -r 'Orllewin' .
```

## MacOS Mimetype

On MacOS you can get a file mime using: `file --mime filename` eg:
```
file --mime 2024_02_17_rhosneigr_beach_anglesey.mp3
2024_02_17_rhosneigr_beach_anglesey.mp3: application/octet-stream; charset=binary
```

## ffmpeg

Turn a still image and an audio file into a video:

```
ffmpeg -loop 1 -i image.jpg -i audio.mp3 -shortest render.mp4
```
## Images/ImageMagick
### Resize and Crop
```bash
convert 'Healing Is A Miracle.jpg' -resize 350x155^ -gravity Center -crop 350x155+0+0 +repage output.jpg
```

### Batch Image Desaturate
Requires ImageMagick. Desaturate all images in a project: `desat.sh`. 

Usage, copy to project folder, pass image extension to convert: `sh desat.sh gif`

Works with animated gifs so useful for removing the sepia tone added to recordings from the [Playdate](https://play.date) simulator.

```bash
#!/bin/bash

find . -name "*.$1" -print0 | while read -d $'\0' file
do
convert -modulate 100,0,100 "$file" "${file%.*}.$1"
done
```

## Switch ssh identities
Pushing to personal and work repos from the same computer, both on Github? You can set this up to be more automatic but I always forget how and it's complicated. This however is simple:

* Add a different cert to each repo
* Add a config file in .ssh: `./.ssh/config`:
```
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_rsa
```
* Edit the IdentityFile for the repo you want to work with
* Then run `ssh-add -D`
