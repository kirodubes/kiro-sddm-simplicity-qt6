<p align="center">
  <img src="kiro.jpg" alt="Kiro" width="220" />
</p>

# edu-sddm-simplicity-qt6

Qt 6 port of the [edu-sddm-simplicity](https://github.com/erikdubois/edu-sddm-simplicity) SDDM theme — clean, minimal login screen, compatible with SDDM builds linked against Qt 6. Part of the `~/EDU/` learning series.

## What's in this repo

- `usr/share/sddm/themes/` — the SDDM theme assets that land in `/usr/share/sddm/themes/`.
- `setup.sh`, `up.sh` — standard EDU bash scaffold.

## Companion variant

- [edu-sddm-simplicity](https://github.com/erikdubois/edu-sddm-simplicity) — Qt 5 / legacy version of the same theme.

## Installation

### From `nemesis_repo` (recommended)

```ini
[nemesis_repo]
SigLevel = Never
Server = https://erikdubois.github.io/$repo/$arch
```

```bash
sudo pacman -Syu
sudo pacman -S edu-sddm-simplicity-qt6
```

You'll also need a Qt 6-linked SDDM:

```bash
sudo pacman -S sddm
sudo systemctl enable sddm.service
```

### Manual

```bash
git clone https://github.com/erikdubois/edu-sddm-simplicity-qt6.git
cd edu-sddm-simplicity-qt6
sudo cp -r usr/share/sddm/themes/. /usr/share/sddm/themes/
```

### Activate

Edit `/etc/sddm.conf` (or drop a file under `/etc/sddm.conf.d/`):

```ini
[Theme]
Current=simplicity
```

Then restart SDDM (or reboot) to see the new login screen.

## Websites

Information : https://erikdubois.be

## Social Media

Youtube : https://www.youtube.com/erikdubois

## License

See [LICENSE](./LICENSE).
