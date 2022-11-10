# Development Documentation on Local VM Testing

- Environment: MacOS M1
- Date Nov 10 2022

## Pre-setup

0. Install `brew`
1. Install `multipass`

```
brew install multipass
```

2. Initialized Ubuntu local VM

```
multipass launch 22.04 --name testbuild --cpus 4 --disk 20G --mem 8G
```

3. Enter into VM

```
multipass shell testbuild
```

4. Add user, make it `sudo`, change user to newly created one

```
sudo adduser whatever
```

```
sudo gpasswd-a whatever sudo
```

```
su whatever
```

## Setup

Setup guide taking reference to [plutus community doc](https://plutus-community.readthedocs.io/en/latest/#Environment/Build/Ubuntufresh/)

1. Update the package and install `curl`

```
sudo sh -c 'apt update && apt install curl'
```

2. Install `git`

```
sudo apt-get install git
```

3. Install `nix`

```
sh <(curl -L https://nixos.org/nix/install) --no-daemon
```

4. Set the environment

```
. /home/whatever/.nix-profile/etc/profile.d/nix.sh
```

5. Adding IOHK caches

```
sudo mkdir ~/.config/nix
echo 'substituters = https://hydra.iohk.io https://iohk.cachix.org https://cache.nixos.org/' >> ~/.config/nix/nix.conf
echo 'trusted-public-keys = hydra.iohk.io:f/Ea+s+dFdN+3Y/G+FDgSq+a5NEWhJGzdjvKNGv0/EQ= iohk.cachix.org-1:DpRUyj7h7V830dp/i6Nti+NEO2/nhblbov/8MW7Rqoo= cache.nixos.org-1:6NCHdD59X431o0gWypbMrAURkbJ16ZPMQFGspcDShjY=' >> ~/.config/nix/nix.conf
```

## Installing the project

1. Create a separate directory

```
mkdir ~/validator-endpoint
```

2. Clone the `plutus-apps`

```
cd ~/validator-endpoint/
git clone https://github.com/input-output-hk/plutus-apps.git
```

3. Clone the Cardano project, here we assume it is open source

```
git clone https://github.com/SIDANWhatever/plutus-cborhex-automation.git
```

4. Entering nix-shell with IOHK binaries, here we checkout latest tag `v1.0.0-alpha1`

```
cd ~/validator-endpoint/plutus-apps
nix-shell
```
