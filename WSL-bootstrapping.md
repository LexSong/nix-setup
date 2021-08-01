# Bootstrapping Nix on WSL

We could use any linux distro with docker-cli. Let's use AlpineWSL here.

## Install [AlpineWSL](https://github.com/yuk7/AlpineWSL) with Docker-CLI

1. Download and extract the [installer](https://github.com/yuk7/AlpineWSL/releases)
2. Run Alpine.exe to extract rootfs and register to WSL
3. Run Alpine.exe again to enter the shell. To add docker-cli using podman, run:
   ```sh
   apk add podman-docker
   ```

## Pull and Export Docker Images from [docker-nixpkgs](https://github.com/nix-community/docker-nixpkgs)

With the docker-cli, run:

```sh
# Pull the latest nix-flakes image
docker pull docker.nix-community.org/nixpkgs/nix-flakes

# Create and export the container
docker export $(docker create nix-flakes) -o ./nix-flakes.tar
```

## Import the exported tar into WSL

In the Windows command line, run:

```batch
wsl --import nix-flakes C:\Users\LexSong\WSL\nix-flakes .\nix-flakes.tar
```

## TODO: Build and Export Docker Images with Nix

## References

- https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro
- https://github.com/nix-community/docker-nixpkgs