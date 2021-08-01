# Bootstrapping Nix on WSL

## Install WSL 2

1. Open the Command Prompt as Administrator and run:

   ```batch
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

   **Restart** the machine to enable WSL and Virtual Machine Platform.

2. Download and install the [WSL2 Linux kernel update package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi).
3. Set WSL 2 as the default version:

   ```batch
   wsl --set-default-version 2
   ```

## Install [AlpineWSL](https://github.com/yuk7/AlpineWSL) with [Podman](https://podman.io)

Let's use AlpineWSL with the daemonless Podman as our Docker replacement:

1. Download and extract the [installer](https://github.com/yuk7/AlpineWSL/releases)
2. Run Alpine.exe to extract rootfs and register to WSL
3. Run Alpine.exe again to enter the shell. To add docker-cli using podman, run:
   ```sh
   apk add podman-docker
   ```

## Pull and Export Docker Images from [docker-nixpkgs](https://github.com/nix-community/docker-nixpkgs)

With the Docker-cli, run:

```sh
# Pull the latest nix-flakes image
docker pull docker.nix-community.org/nixpkgs/nix-flakes

# Create and export the container
docker export $(docker create nix-flakes) -o ./nix-flakes.tar
```

## Import the Exported Tar into WSL

In the Windows command line, run:

```batch
wsl --import nix-flakes C:\Users\LexSong\WSL\nix-flakes .\nix-flakes.tar
```

**TODO:** We need to export environment variables from the docker image to the WSL filesystem.

## TODO: Build and Export Docker Images with Nix

## References

- [WSL 2 Installation Guide](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
- [Import any Linux distribution to use with WSL](https://docs.microsoft.com/en-us/windows/wsl/use-custom-distro)
- [docker-nixpkgs: docker images from nixpkgs](https://github.com/nix-community/docker-nixpkgs)
