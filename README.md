# ansible-debian-gnome-desktop

Ansible playbook for setup Debian GNOME Desktop

Features:

- Setup Debian Stable with backports and Flatpak apps
- Setup Debian Sid (Unstable) with LTS Linux kernel
- Install non-free components or free only components

### Before start

Make sure you have installed:

- ansible
- git

```
sudo apt install git ansible
```

Then install required Ansible collections:

```
ansible-galaxy install -r requirements.yml
```

### Pull from the repo

```sh
ANSIBLE_FORCE_COLOR=1 ansible-pull -U https://github.com/SmartFinn/ansible-debian-gnome-desktop.git --ask-become-pass
```

### Run locally

```sh
git pull https://github.com/SmartFinn/ansible-debian-gnome-desktop.git
cd ansible-debian/
ansible-playbook local.yml --ask-become-pass
```

### Optional variables

The optional variable may be set with `-e var=value` options to `ansible-pull` or `ansible-playbook` commands.

- `hostname` - specify hostname in FQDN format (e.g. `-e hostname=thinkpad.mkro.tk`)
- `timezone` - change timezone specified during install (e.g. `-e timezone=Europe/Kyiv`)
- `modern_intel_driver` - choose between mature `libva-intel-driver` (default) and modern `intel-media-driver` (e.g. `-e modern_intel_driver=true`)
- `flatpak_install_method` - install Flatpak apps for the whole system (default) or only for the current user (e.g. `-e flatpak_install_method=user`)
- `xorg_tearfree` - enable this only if you have issues with tearing in X11 session (e.g. `-e xorg_tearfree=true`)
- `extra_packages` - list of packages to install (e.g. `-e "extra_packages=['strace','rpmconf']"`)
- `surfshark_username` - username for Surfshark VPN connection (required by `surfshark-ikev2` task)
- `surfshark_password` - password for Surfshark VPN connection (required by `surfshark-ikev2` task)
- `surfshark_servers` - list of Surfshark servers (e.g. `-e "surfshark_servers=['uk-lon.prod.surfshark.com','ua-iev.prod.surfshark.com']"`)
- `zerotier_network_id` - join ZeroTier network if the variable is defined (e.g. `-e zerotier_network_id=8056c2e21c000001`)
- `debian_sid` - install packages and upgrade the system to Debian Unstable (e.g. `-e debian_sid=true`)
- `debian_mirror` - specify Debian mirror for /etc/apt/sources.list (e.g. `-e debian_mirror=debian.csail.mit.edu`)
- `non_free` - install packages with non-free components (default) (e.g. `-e non_free=false`)
- `kernel_lts` - do not install newer Linux kernels from backports and sid (default) (e.g. `-e kernel_lts=false`)
- `force_flatpak` - using Flatpak instead of native packages from third-party repos if there is a choice (e.g. `-e force_flatpak=true`)

### Optional tags

Tags are used to enable/disable tasks in the playbook.<br/>
To run only specific tasks use `--tags tag1,tag2,tagN` option and to exclude tasks use `--skip-tags tag1,tag2,tagN`.

#### System

| Tag | Enabled | Description |
|:----|:--------|:------------|
| `system` | Yes | Master tag for *system* category. Use `--tags system` with `--skip-tags never` to skip unnecessary tasks |
| `flatpak` | Yes | Install and configure Flatpak |

#### Network

| Tag | Enabled | Description |
|:----|:--------|:------------|
| `network` | Yes | Master tag for *network* category. Use `--tags network` with `--skip-tags never` to skip unnecessary tasks |
| `surfshark` | No | Add Surfshark IKEv2 connection to Network Manager |
| `zaborona` | No | Add [zaborona.help](zaborona.help) OpenVPN connection to Network Manager |
| `vpn` | No | Enable `surfshark-ikev2` and `zaborona-openvpn` tasks |

#### Hardware

| Tag | Enabled | Description |
|:----|:--------|:------------|
| `hardware` | Yes | Master tag for *hardware* category. Use `--tags hardware` with `--skip-tags never` to skip unnecessary tasks |
| `intel-video` | Yes | Setup VA-API, tools for Intel Graphics.Tweak `i915` module and Xorg |
| `intel-audio` | Yes | Tweak `snd_hda_intel` module |
| `synaptics-touchpad` | No | Setup Synaptics Touchpad instead of libinput |
| `logitech-trackball` | No | Tweak Logitech Trackball M570 |

#### GNOME

| Tag | Enabled | Description |
|:----|:--------|:------------|
| `gnome` | Yes | Master tag for *gnome* category. Use `--tags gnome` with `--skip-tags never` to skip unnecessary tasks |
| `gnome-shell-extensions` | Yes | Install and configure GNOME Shell extensions |
| `gnome-apps-settings` | Yes | Configure GNOME Apps |
| `gnome-settings` | Yes | General GNOME settings |
| `gnome-appearance` | Yes | Install and set themes |
| `gnome-gestures` | No | Enable GNOME touchpad gestures on X11 |
| `gdm-settings` | Yes | Configure GDM to act like GNOME Desktop |
| `user-icon` | No | Set icon for the user account |

#### General

| Tag | Enabled | Description |
|:----|:--------|:------------|
| `btrfs-subvolid0` | No | Create .automount for mount BTRFS root subvolume |
| `dotfiles` | No | Download and install dotfiles |

#### Apps

| Tag | Description |
|:----|:------------|
| `apps` | Install all apps from all available tasks |
| `cli` | Apps and utils with TUI or a readline interface |
| `dev` | Tools for development |
| `desktop` | Common apps and utils |
| `flatpak` | All Flatpak available packages |
| `xorg-extras` | Utils for X11 clipboard or windows manage |

| Tag | Group(s) | Description |
|:----|:---------|:------------|
| `backintime` | `desktop` | Backup home directory to external USB drive/home server |
| `barrier` | — | Open-source KVM software |
| `brave-browser` | — | Add repository and install Brave Browser |
| `btdu` | `cli` | Download and install the latest btdu from GitHub |
| `btrfs-list` | `cli` | Download and install the latest btrfs-list from GitHub |
| `crow-translate` | `desktop`, `flatpak` | A simple and lightweight translator |
| `digikam` | `desktop`, `flatpak` | The photo manager |
| `easyeffects` | — | Audio effects for PipeWire applications |
| `firefox` | — | Install latest Firefox instead of Firefox ESR |
| `flameshot` | `desktop` | Install Flameshot and setup keybindings |
| `flatseal` | `desktop`, `flatpak` | Manage Flatpak permissions |
| `gimp` | `desktop`, `flatpak` | Create images and edit photographs |
| `gnome-extras` | `desktop` | GNOME apps and utils for GNOME Desktop |
| `google-chrome` | — |  Add repository and install Google Chrome |
| `google-cloud-cli` | — | Add repository and install gcloud utility |
| `google-earth-pro` | `flatpak` | Add repository and install Google Earth Pro |
| `gpick` | `desktop` | Advanced color picker |
| `gpxsee` | `desktop` | GPS log file viewer and analyzer |
| `imv` | `desktop` | Image viewer for X11 and Wayland |
| `inkscape` | `desktop`, `flatpak` | Vector-based drawing program using SVG |
| `josm` | `flatpak` | Java OpenStreetMap Editor |
| `keepassxc` | `desktop` | Cross-platform password manager |
| `kitty` | `desktop` | Install Kitty terminal |
| `libvirt` | — | Alias to `virt-manager` |
| `meld` | `dev` | Visual diff and merge tool |
| `mpv` | `desktop` | Movie player playing most video formats |
| `neovim` | `dev` | Add repository and install Neovim |
| `obsidian` | `desktop`, `flatpak` | Markdown-based knowledge base |
| `peek` | — | Animated GIF screen recorder with an easy to use interface |
| `qbittorrent` | `desktop` | A Bittorrent Client |
| `qgis` | `flatpak` | A Free and Open Source Geographic Information System |
| `papirus-folders` | — | Install papirus-folders script |
| `rofi` | `desktop` | Install Rofi launcher and setup keybindings |
| `scrcpy` | — | Install scrcpy (Debian Sid only) |
| `signal` | `desktop`, `flatpak` | Private messenger |
| `spotify` | `desktop`, `flatpak` | Online music streaming service |
| `sqlitebrowser` | `flatpak` | DB Browser for SQLite is a light GUI editor for SQLite databases |
| `syncthing` | `desktop` | Continuous File Synchronization |
| `telegram-desktop` | `desktop`, `flatpak` | A messaging app with a focus on speed and security |
| `timeshift` | — | System restore tool for Linux |
| `virt-manager` | — | Install virt-manager, add the user to libvirt group, and add .kvm domain name to hosts |
| `vscode` | `dev` | Add repository and install Visual Studio Code |
| `wine` | `flatpak` | Run Windows applications on Linux |
| `wireshark` | — | Network traffic analyzer |
| `zeal` | `dev` | Offline documentation browser inspired by Dash |
| `zerotier` | — | Add repository and install ZeroTier |
| `zsh` | `cli` | Powerful interactive shell |
