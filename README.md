# rpi-chromium-mods

This is our "fork" of Raspberry Pi's `rpi-chromium-mods` debian repository.

**Base Package Last Updated: `Kano OS v4.3.0`**
**Kano Customisations Last Updated: `Kano OS v4.3.2`**


## Updating rpi-chromium-mods

In Kano OS, we mirror Rasbian and Raspberry Pi debian sources. Therefore, when
we update the Raspberry Pi mirrors, we get a new version of the original
rpi-chromium-mods. This newer version will always be overshadowed by Kano's
`rpi-chromium-mods-kano`.

As such, in order to update the package, you'll simply need to unpack the newer
version of rpi-chromium-mods and add the files to this repository.


## Commit Conventions

From 5 April 2019, the contributions to this project will follow these rules:

  - [Our single commit which adds everything on top of a default version]
  - [Base version of rpi-chromium-mods]
  - [Base version of rpi-chromium-mods]
  - ...
  - [Added .gitignore and README.md](https://github.com/KanoComputing/rpi-chromium-mods/commit/6160f147a9205ea9e40353967acdd3205db3cd30)
  - [Reset project, start fresh](https://github.com/KanoComputing/rpi-chromium-mods/commit/8570389a27dd6a96eb61e500671351b809735b84)

This means that when a newer version of rpi-chromium-mods is added, the commit
will be added below our customisations and you'll have to update those changes.

When commiting a newer version of rpi-chromium-mods, do so **using the commit
message convention** exemplified in [7c715e74381082f0d5dba7b3719dfe4c2a568807](https://github.com/KanoComputing/rpi-chromium-mods/commit/7c715e74381082f0d5dba7b3719dfe4c2a568807).


## Example (macOS)

#### 1. Create a temporary workspace

```
mkdir -p /tmp/rpi-chromium-mods/
cd /tmp/rpi-chromium-mods/
```

#### 2. Download the new package locally

```
wget http://archive.raspberrypi.org/debian/pool/ui/r/rpi-chromium-mods/rpi-chromium-mods_20190103_armhf.deb
```

#### 3. Unpack the debian package

```
ar x rpi-chromium-mods_20190103_armhf.deb
tar -zxvf *.tar.gz
tar -xvf *.tar.xz
cd usr/share/doc/rpi-chromium-mods/ && gunzip *.gz && cd ../../../..
rm *.deb *.tar.gz *.tar.xz
```

#### 4. In the repository, revert the customisations commit and stash it

```
cd ~/Kano/rpi-chromium-mods/
git checkout -b update
git reset HEAD~1
git stash
```

#### 5. Copy all the existing and new files from the new rpi-chromium-mods

```
cp /tmp/rpi-chromium-mods/usr/lib/chromium-browser/* armhf/
cp /tmp/rpi-chromium-mods/etc/chromium-browser/customizations/* armhf/
cp /tmp/rpi-chromium-mods/{config,control,postinst,postrm,templates} debian/
cp /tmp/rpi-chromium-mods/usr/share/doc/rpi-chromium-mods/* debian/
```

There may other files, so pay attention here.

#### 6. Commit the newer version of rpi-chromium-mods

```
git commit
```

See `Commit Conventions` for what to use as the commit message.

#### 7. Apply Kano customisations on top

```
git stash pop
```

#### 8. Solve any issues there may be with the new version

#### 9. Commit the new Kano customisation single commit on top

```
git add *
git commit
```

Use the same commit message (or update as necessary):

```
Project customisations for Kano OS

This single commit is always applied on top of the latest mirror
of rpi-chromium-mods and it contains all extras specific to Kano OS.
To view previous versions of this package, see Tags. Our changes
are only in packaging and Chromium's master_preferences file.
```

#### 10. Update this README with the version of the OS this is for

#### 11. Push your changes

Because the history has been rewritten, you'll need to push with force.

```
git push origin update --force-with-lease
```
