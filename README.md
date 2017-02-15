# kinesalite-nexe

A convenient single binary for [Kinesalite](https://github.com/mhart/kinesalite) built using [nexe](https://github.com/nexe/nexe). The resulting binary has no runtime dependencies on `npm` / `node`.

## Build

```shell
docker run -it -v $(pwd):/mnt amazonlinux /bin/bash -c "$(cat build.sh)"
```

## Run

To run with arguments, a slight invocation change is required to accomodate `npm` args added by the `nexe` build:

```bash
./kinesalite <npm-args> -- <args>
```

## Deploy

Copy to `dist/<version>/<tarball>` and commit
