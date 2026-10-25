# Vacuula for Unraid

Community-maintained Unraid Docker template for the official
[Vacuula](https://github.com/vacuula/vacuula) image, configured for Neato Botvac
**D3, D4, D5, D6 and D7** (Gen3).

## Beta channel

**This template tracks `ghcr.io/vacuula/vacuula:beta`.** Vacuula is under active
development. Updates can introduce bugs, breaking changes or database migrations.
Back up the complete appdata directory before updating. Rolling back an image
after a database migration may also require restoring its matching data backup.

The template does not enable unattended updates. Updates follow your Unraid and
update-plugin settings. The bridge/network configuration was tested with upstream
1.1.0-b7; a moving beta tag can contain a newer version that has not been separately
validated by this template's maintainer. Check upstream changes before updating.

## Install

If this repository is available in Community Applications, search for **vacuula**.
Otherwise copy [`templates/vacuula.xml`](templates/vacuula.xml) into
`/boot/config/plugins/dockerMan/templates-user/my-vacuula.xml` on your Unraid
server, then select it under **Docker → Add Container → User templates**.
Back up an existing template before replacing it.

Select `bridge` or an existing custom Docker bridge network. Set the appdata path,
timezone, and a free web host port. Enable Autostart in Unraid if desired.

| Setting | Default |
|---|---|
| Docker image | `ghcr.io/vacuula/vacuula:beta` |
| Network | `bridge`; custom bridge networks supported |
| Web/API/installer | Host TCP 8088 → container TCP 8088 |
| Direct Gen3 robot connection | Host TCP 335 → container TCP 335 |
| Persistent storage | `/mnt/user/appdata/vacuula` → `/server/data` |
| Timezone | `Europe/Prague`; change for your location |
| Bind address inside the container | `0.0.0.0` |
| mDNS | Disabled; enter the server URL manually |
| Gen4 | Disabled; this template targets D3–D7 |

The general template publishes these two ports on host interfaces. Restrict access
using your network configuration as appropriate; this is a local backend and no
Internet port forwarding is required. Do not publish duplicate port rules.

## Enroll the robot

1. Start the container and open its WebUI.
2. Run the official [Vacuula Installer](https://github.com/vacuula/installer/releases/latest)
   on a separate computer with USB and Wi-Fi.
3. Enter the Unraid LAN IP and the published web port as the server URL, for example
   `http://UNRAID_LAN_IP:8088`. mDNS discovery is disabled in this template.
4. Follow the installer. It modifies robot firmware to connect to your own server.
   No USB device needs to be passed through to the Docker container.

Keep TCP 335 identical on both sides of the robot mapping. Do not put this robot
port behind an HTTP reverse proxy. The web host port may be changed independently;
keep its container target and `VACUULA_PORT` at 8088.

Keep the server's LAN IP stable. The robot must be able to reach the published
ports. See the [upstream FAQ](https://github.com/vacuula/vacuula/blob/main/FAQ.md)
for Gen3 DNS/nip.io, Internet-check and time-service requirements.

Home Assistant is not required. Running the backend does not by itself provide
a Home Assistant vacuum integration. Gen4 robots D8–D10 are not configured by this
template, even though the upstream application supports them.

## Preserve appdata

`/server/data` contains the database, maps, TLS certificate and its private key.
Preserve and back up the entire directory, especially for robots already enrolled.
Do not run two instances against the same data directory. Docker logs are limited
to three 10 MB files; this template does not configure automatic data backups.

## Support and attribution

- [Template issues](https://github.com/Desadius/unraid-vacuula/issues): Unraid template configuration.
- [Vacuula project](https://github.com/vacuula/vacuula): backend, robot support and installer.

This repository is independent of the Vacuula authors. It distributes configuration
and documentation, not a modified application image. The application and linked
Vacuula artwork retain their upstream licenses. Template code and documentation
in this repository are provided under the MIT license.

Submission to Community Applications does not imply approval or immediate listing.
