# easy-novnc

A single portable binary that serves the [noVNC](https://github.com/novnc/noVNC) web UI and a WebSocket-to-VNC proxy.

**This project does not embed or ship a VNC server** (no TightVNC, x11vnc, RealVNC, etc.). Point it at an **existing** VNC host and port.

Fork of the archived MIT project [pgaskin/easy-novnc](https://github.com/pgaskin/easy-novnc).

## What it does

| Included | Not included |
| --- | --- |
| noVNC browser UI (embedded) | VNC server |
| WebSocket → TCP VNC proxy | Desktop sharing / screen capture |
| Optional CIDR allow/deny lists | TightVNC / x11vnc installers |

You still need a VNC server running elsewhere (for example on the machine you want to view). easy-novnc only connects to that server and exposes it in a browser.

## Features

- Clean start page
- CIDR whitelist / blacklist
- Optionally allow connections to arbitrary hosts (and ports)
- Checks the target speaks RFB so non-VNC ports are not tunneled
- Configure with flags or environment variables (works out of the box)
- IPv6 support
- Single static binary, no runtime dependencies
- Optional [wstcp](./wstcp) client for local TCP over WebSockets

## Build from source

Requires Go 1.14+ (CI uses Go 1.22).

```bash
git clone https://github.com/weiwan-gmail/easy-novnc.git
cd easy-novnc

# native
go build -o easy-novnc .

# cross-compile (same commands used in CI)
GOOS=linux   GOARCH=amd64 go build -o easy-novnc-linux-amd64 .
GOOS=windows GOARCH=amd64 go build -o easy-novnc-windows-amd64.exe .
```

Release assets `easy-novnc-linux-amd64` and `easy-novnc-windows-amd64.exe` are published automatically when a tag matching `v*` is pushed (see `.github/workflows/build.yml`).

## Run (connect to an existing VNC server)

Assume a VNC server is already listening on `192.168.1.50:5900`:

```bash
./easy-novnc-linux-amd64 -a :8080 -h 192.168.1.50 -p 5900
# open http://localhost:8080 in a browser
```

On Windows:

```bat
easy-novnc-windows-amd64.exe -a :8080 -h 192.168.1.50 -p 5900
```

Default listen address is `:8080`; default VNC target is `localhost:5900`.

## Usage

```
Usage: easy-novnc [options]

Options:
  -a, --addr string              The address to listen on (env NOVNC_ADDR) (default ":8080")
  -H, --arbitrary-hosts          Allow connection to other hosts (env NOVNC_ARBITRARY_HOSTS)
  -P, --arbitrary-ports          Allow connections to arbitrary ports (requires arbitrary-hosts) (env NOVNC_ARBITRARY_PORTS)
  -u, --basic-ui                 Hide connection options from the main screen (env NOVNC_BASIC_UI)
  -C, --cidr-blacklist strings   CIDR blacklist for when arbitrary hosts are enabled (comma separated) (conflicts with whitelist) (env NOVNC_CIDR_BLACKLIST)
  -c, --cidr-whitelist strings   CIDR whitelist for when arbitrary hosts are enabled (comma separated) (conflicts with blacklist) (env NOVNC_CIDR_WHITELIST)
      --default-view-only        Use view-only by default (env NOVNC_DEFAULT_VIEW_ONLY)
      --help                     Show this help text
  -h, --host string              The host/ip to connect to by default (env NOVNC_HOST) (default "localhost")
      --no-url-password          Do not allow password in URL params (env NOVNC_NO_URL_PASSWORD)
  -p, --port uint16              The port to connect to by default (env NOVNC_PORT) (default 5900)
  -v, --verbose                  Show extra log info (env NOVNC_VERBOSE)
```

## License and attribution

MIT License — see [LICENSE.md](./LICENSE.md).

Upstream: [pgaskin/easy-novnc](https://github.com/pgaskin/easy-novnc) (MIT, archived). This fork keeps the MIT license and upstream credit; module path remains `github.com/pgaskin/easy-novnc` for minimal churn.
