# README

Restic backup program

## Installation

```sh
# Define installation folder

export INSTALL_DIRECTORY=/usr/bin

# Use local installation

sudo bin/installer install

# Use remote installation

curl --location "https://gitlab.com/timonier/restic/raw/master/bin/installer" | sudo sh -s -- install
```

__Note 1__: If you do not define `INSTALL_DIRECTORY`, `installer` will use in `/usr/local/bin`.

__Note 2__: `docker-for-mac` users have to configure [native NFS server](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc).

## Usage

### rclone

Run the command `rclone`:

```sh
rclone config
# Current remotes:
#
# Name                 Type
# ====                 ====
# drive                drive
#
# e) Edit existing remote
# n) New remote
# d) Delete remote
# r) Rename remote
# c) Copy remote
# s) Set configuration password
# q) Quit config
# e/n/d/r/c/s/q> r
# Choose a number from below, or type in an existing value
#  1 > drive
# remote> 1
# Enter new name for "drive" remote.
# name> new-drive
# Current remotes:
#
# Name                 Type
# ====                 ====
# new-drive            drive
#
# e) Edit existing remote
# n) New remote
# d) Delete remote
# r) Rename remote
# c) Copy remote
# s) Set configuration password
# q) Quit config
# e/n/d/r/c/s/q> q
```

### rest-server

Run the command `rest-server`:

```sh
# See all rest-server options

rest-server --help

# Run rest-server

rest-server --path "${HOME}"/backup
# rest-server 0.9.0 (ed8b484) compiled with go1.7.3 on linux/amd64
# Data directory: /home/morgan/backup
# Authentication disabled
# Starting server on :8000

# In another terminal, run restic

export RESTIC_PASSWORD=my-super-password
export RESTIC_REPOSITORY=rest:"http://127.0.0.1:8000"

restic init
# created restic backend 1e491e8100 at rest:http://127.0.0.1:8000
#
# Please note that knowledge of your password is required to access
# the repository. Losing your password means that your data is
# irrecoverably lost.

restic backup --one-file-system "${HOME}"/Bureau
# scan [/home/morgan/Bureau]
# scanned 1 directories, 8 files in 0:00
# [0:00] 100.00%  0B/s  329.613 KiB / 329.613 KiB  9 / 9 items  0 errors  ETA 0:00
# duration: 0:00, 10.65MiB/s
# snapshot 2035783c saved
```

### restic

Run the command `restic`:

```sh
# See all restic options

restic --help

# Run restic

export RESTIC_PASSWORD=PASSWORD
export RESTIC_REPOSITORY="${HOME}"/backup

restic init
# created restic backend e56e03fad8 at /home/morgan/backup
#
# Please note that knowledge of your password is required to access
# the repository. Losing your password means that your data is
# irrecoverably lost.

restic backup --one-file-system "${HOME}"/Bureau
# scan [/home/morgan/Bureau]
# scanned 1 directories, 8 files in 0:00
# [0:00] 100.00%  0B/s  329.613 KiB / 329.613 KiB  9 / 9 items  0 errors  ETA 0:00
# duration: 0:00, 10.23MiB/s
# snapshot 8f22690c saved

restic snapshots
# ID        Date                 Host        Tags        Directory
# ----------------------------------------------------------------------
# 8f22690c  2017-04-28 09:38:37  dscb004273              /home/morgan/Bureau

restic restore 8f22690c --target "${HOME}"
# restoring <Snapshot 8f22690c of [/home/morgan/Bureau] at 2017-04-28 09:38:37.240551869 +0200 CEST by morgan@dscb004273> to /home/morgan
```

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a [merge request](https://docs.gitlab.com/ee/user/project/merge_requests/).

__Note 1__: [GitHub repository](https://github.com/timonier/restic) is a mirror. [Merge request](https://docs.gitlab.com/ee/user/project/merge_requests/) has to be submitted to the [GitLab repository](https://gitlab.com/timonier/restic).

__Note 2__: Use the script `bin/build-image` to test your modifications locally.

If you like / use this project, please let me known by adding a [★](https://help.github.com/articles/about-stars/) on the [GitHub repository](https://github.com/timonier/restic) or on the [GitLab repository](https://gitlab.com/timonier/restic).

## Links

* [image "timonier/restic"](https://hub.docker.com/r/timonier/restic/)
* [ncw/rclone](https://github.com/ncw/rclone)
* [restic/rest-server](https://github.com/restic/rest-server)
* [restic/restic](https://github.com/restic/restic)
* [set up docker for mac with native nfs](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc)
* [timonier/dumb-entrypoint](https://gitlab.com/timonier/dumb-entrypoint)
* [timonier/version-lister](https://gitlab.com/timonier/version-lister)
