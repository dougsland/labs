Upgrade container images using static delta approach.
Since we know what image we want to upgrade and the version we want to
upgrade to; also since we don't need to go through a registry, we can
more easily use any kind of data we want.

This tool uses [podman](https://github.com/containers/podman) and [tardiff](https://github.com/containers/tar-diff)

```
$ podman pull docker.io/nginx:1.24.0 docker.io/nginx:1.25
### create the tardiff under /tmp/diff
$ ./tardiff-poc diff docker.io/nginx:1.24.0 docker.io/nginx:1.25.0 /tmp/diff
### create the tardiff apply the diff from /tmp/diff to the current storage
$ ./tardiff-poc apply ~/.local/share/containers/storage docker.io/nginx:1.24.0 /tmp/diff /tmp/resulting-image

### try to pull the resulting image from a different user (and empty storage)

$ sudo podman pull dir:/tmp/resulting-image
Getting image source signatures
Copying blob 0cc1f0165626 done   |
Copying blob 077db2bd2c24 done   |
Copying blob cc7def5d7708 done   |
Copying blob 89ad618cc7b9 done   |
Copying blob f096f2cad7ff done   |
Copying blob 5f5ffeb5f485 done   |
Copying config 7d3c40f240 done   |
Writing manifest to image destination
7d3c40f240e18f6b440bf06b1dfd8a9c48a49c1dfe3400772c3b378739cbdc47w
$ sudo podman tag 7d3c40f240e18f6b440bf06b1dfd8a9c48a49c1dfe3400772c3b378739cbdc47 docker.io/library/nginx:1.25.0

```
