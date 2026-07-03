# project-zomboid-server-docker-arm64

Project Zomboid dedicated server Docker image for ARM64.

This is a fork of [joyfuI/project-zomboid-server-docker-arm64](https://github.com/joyfuI/project-zomboid-server-docker-arm64), maintained by [@rm968211](https://github.com/rm968211). Full credit to the original author for the ARM64/FEX-Emu approach.

The image uses [FEX-Emu](https://fex-emu.com/) to run the x86 Project Zomboid server binary on ARM64 hardware. This is necessary because Valve does not provide native ARM64 server binaries for Project Zomboid.

---

## Usage

```bash
docker run -d \
  --name project-zomboid-server \
  -p 16261:16261/udp \
  -p 16262:16262/udp \
  -v <zomboid-folder>:/home/steam/Zomboid \
  -v <workshop-folder>:/home/steam/pzserver/steamapps/workshop \
  -e TZ=America/New_York \
  -e SERVERNAME=servertest \
  --restart unless-stopped \
  rm968211/project-zomboid-server-docker-arm64:latest
```

On first run, the server binary will be downloaded via SteamCMD. This may take several minutes. The server will not be accessible until the download completes.

Once the container is running, attach to the server console:

```bash
docker exec -it project-zomboid-server /bin/bash
./run.sh
```

You must set an admin password on first launch via the in-game console. After setting it, detach from the screen session with `Ctrl+A, D` to leave the server running in the background.

If the server crashes or exits, it will automatically restart after 10 seconds.

---

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `TZ` | — | Timezone (e.g. `America/New_York`) |
| `SERVERNAME` | `servertest` | Server name / save directory |
| `CPU_MHZ` | `2000` | Hint to FEX-Emu for CPU frequency |

## Volumes

| Path | Description |
|---|---|
| `/home/steam/Zomboid` | Server data — saves, config, logs |
| `/home/steam/pzserver/steamapps/workshop` | Workshop mod files |

---

## Notes

- On first launch, the admin password must be set interactively via the console (`./run.sh`). There is currently no way to pre-set it via environment variable.
- The server runs inside a `screen` session. Use `./run.sh` to attach, and `Ctrl+A, D` to detach without stopping it.
- Server config reference: [PZWiki — Dedicated Server](https://pzwiki.net/wiki/Dedicated_server)
