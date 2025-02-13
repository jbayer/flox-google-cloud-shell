# Flox on Goolge Cloud Shell
Instructions for using Flox on Google Cloud Shell.

Steps 1 and 2 below need to be repeated each time you start a new Google Cloud Shell VM. This is because the shell only has 5GB of storage in the user's local home directory, which persists between sessions. All other storage (where Nix and Flox are installed) is removed when the Shell VM stops.

Nix and Flox files that represent environments persist if they are in the home directory.

## 1. Install Nix in Single User mode

```bash
# installs nix locally
sh <(curl -L https://nixos.org/nix/install) --no-daemon

# create an /etc/nix/nix.conf
sudo mkdir /etc/nix
sudo vi /etc/nix/nix.conf
```

Add these lines to `/etc/nix/nix.conf`:

```bash
experimental-features = nix-command flakes
extra-trusted-substituters = https://cache.flox.dev
extra-trusted-public-keys = flox-cache-public-1:7F4OyH7ZCnFhcze3fJdfyXYLQw/aV7GEed86nQ7IsOs=
```

Source the Nix profile:

```bash
. ~/.nix-profile/etc/profile.d/nix.sh
```

## 2. Install Flox

Install Flox to your personal user profile:

```bash
nix profile install \
  --experimental-features "nix-command flakes" \
  --accept-flake-config \
  'github:flox/flox'
```

Verify the installation:

```bash
flox --version
```

## 3. Use Flox default environment, install a nix package, use it

```bash
# initialize flox
flox init

# install hello, which is from nixpkgs
flox install hello

# activate flox
flox activate

# run hello
hello
```

Here is the full output example:
```bash
jambay@cloudshell:~$ pwd
/home/jambay
jambay@cloudshell:~$ flox init
Flox collects basic usage metrics in order to improve the user experience.

Flox includes a record of the subcommand invoked along with a unique token.
It does not collect any personal information.

The collection of metrics can be disabled in the following ways:

  environment: FLOX_DISABLE_METRICS=true
    user-wide: flox config --set-bool disable_metrics true
  system-wide: update /etc/flox.toml as described in flox-config(1)

This is a one-time notice.


✨ Created environment 'default' (x86_64-linux)

Next:
  $ flox search <package>    <- Search for a package
  $ flox install <package>   <- Install a package into an environment
  $ flox activate            <- Enter the environment
  $ flox edit                <- Add environment variables and shell hooks

jambay@cloudshell:~$ flox install hello
✅ 'hello' installed to environment 'default'
jambay@cloudshell:~$ flox activate
✅ You are now using the environment 'default'.
To stop using this environment, type 'exit'

jambay@cloudshell:~$ hello
Hello, world!
```
