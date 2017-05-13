# README

Restic backup program

## Installation

Copy `rest-server`, `bin/restic` and `restic-cron` into your executable folder (like `/usr/local/bin` or `$HOME/bin`):

```sh
sudo curl --location --output /usr/local/bin/rest-server "https://github.com/timonier/restic/raw/master/bin/rest-server"
sudo chmod +x /usr/local/bin/rest-server

sudo curl --location --output /usr/local/bin/restic "https://github.com/timonier/restic/raw/master/bin/restic"
sudo chmod +x /usr/local/bin/restic

sudo curl --location --output /usr/local/bin/restic-cron "https://github.com/timonier/restic/raw/master/bin/restic-cron"
sudo chmod +x /usr/local/bin/restic-cron
```

Linux users can use the [installer](https://github.com/timonier/restic/blob/master/bin/installer):

```sh
curl --location "https://github.com/timonier/restic/raw/master/bin/installer" | sudo sh -s install
```

## Usage

### rest-server

Run the command `rest-server`:

```sh
# See all rest-server options

rest-server --help

# Run rest-server

rest-server --path "${HOME}/backup"
# rest-server 0.9.0 (ed8b484) compiled with go1.7.3 on linux/amd64
# Data directory: /home/morgan/backup
# Authentication disabled
# Starting server on :8000

# In another terminal, run restic

export RESTIC_PASSWORD="PASSWORD"
export RESTIC_REPOSITORY="rest:http://127.0.0.1:8000"

restic init
# created restic backend 1e491e8100 at rest:http://127.0.0.1:8000
#
# Please note that knowledge of your password is required to access
# the repository. Losing your password means that your data is
# irrecoverably lost.

restic backup --one-file-system "${HOME}/Bureau"
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

export RESTIC_PASSWORD="PASSWORD"
export RESTIC_REPOSITORY="${HOME}/backup"

restic init
# created restic backend e56e03fad8 at /home/morgan/backup
#
# Please note that knowledge of your password is required to access
# the repository. Losing your password means that your data is
# irrecoverably lost.

restic backup --one-file-system "${HOME}/Bureau"
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

### restic-cron

Run the command `restic-cron`:

```sh
export RESTIC_KEEP_LAST=1
export RESTIC_PASSWORD="PASSWORD"
export RESTIC_REPOSITORY="${HOME}/backup"
export RESTIC_SCHEDULE="* * * * *"

restic-cron "${HOME}/Bureau"
# create backend at /home/morgan/backup failed: config file already exists
#
# Next run at 2017-04-28 09:43:00 +0200 CEST Sleeping for 37.309852306s
#
# using parent snapshot 8f22690c
# scan [/home/morgan/Bureau]
# scanned 1 directories, 8 files in 0:00
# [0:00] 100.00%  0B/s  329.613 KiB / 329.613 KiB  9 / 9 items  0 errors  ETA 0:00
# duration: 0:00, 11.72MiB/s
# snapshot 8e6240d1 saved
#
# snapshots for host dscb004273, paths: [/home/morgan/Bureau]:
#
# keep 1 snapshots:
# ID        Date                 Host        Tags        Directory
# ----------------------------------------------------------------------
# 8e6240d1  2017-04-28 09:43:00  dscb004273              /home/morgan/Bureau
#
# remove 1 snapshots:
# ID        Date                 Host        Tags        Directory
# ----------------------------------------------------------------------
# 8f22690c  2017-04-28 09:38:37  dscb004273              /home/morgan/Bureau
#
# 1 snapshots have been removed, running prune
# counting files in repo
# building new index for repo
# [0:00] 100.00%  3 / 3 packs
# repository contains 3 packs (12 blobs) with 336.227 KiB bytes
# processed 12 blobs: 0 duplicate blobs, 0B duplicate
# load all snapshots
# find data that is still in use for 1 snapshots
# [0:00] 100.00%  1 / 1 snapshots
# found 10 of 12 data blobs still in use, removing 2 blobs
# will delete 0 packs and rewrite 2 packs, this frees 2.914 KiB
# [0:00] 100.00%  2 / 2 packs rewritten
# counting files in repo
# [0:00] 100.00%  2 / 2 packs
# finding old index files
# saved new index as 16a4737d
# remove 2 old index files
# done

# Next run at 2017-04-28 09:44:00 +0200 CEST Sleeping for 58.43000103s
```

__Note 1__: Only environment variables `RESTIC_PASSWORD` and `RESTIC_REPOSITORY` are mandatory.

__Note 2__: By default, `restic-cron` will run once a day at midnight. It is possible to change the [scheduling](https://en.wikipedia.org/wiki/Cron) via the environment variable `RESTIC_SCHEDULE`.

__Note 3__: By default, `restic-cron` will keep the `7` last snapshots. It is possible to change this number via the environment variable `RESTIC_KEEP_LAST`.

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a pull request.

__Note__: Use the script `bin/build` to test your modifications locally.

## Links

* [cron](https://en.wikipedia.org/wiki/Cron)
* [restic/rest-server](https://github.com/restic/rest-server)
* [restic/restic](https://github.com/restic/restic)
* [image "timonier/restic"](https://hub.docker.com/r/timonier/restic/)
* [timonier/dumb-entrypoint](https://github.com/timonier/dumb-entrypoint)
* [timonier/version-lister](https://github.com/timonier/version-lister)
* [transitorykris/hypnos](https://github.com/transitorykris/hypnos)
