# V2Ray Docker Compose

This repository introduces two V2Ray-based solutions to bypass censorship in highly restricted networks
without direct access to an upstream server (a server with access to free Internet).
These solutions are 1. Using a bridge server, and 2. Using a CDN service.

## Documentation

### Setup V2Ray Using a Bridge Server

In this solution, you need these two servers:

* Upstream Server: A server that has access to the free Internet.
* Bridge Server: A server that is available to clients and has access to the upstream server.

```
(Client) <-> [ Bridge Server ] <-> [ Upstream Server ] <-> (Internet)
```

This solution consists of two steps and provides original VMESS and Shadowsocks (AEAD) protocols.

#### Setup Upstream Server

1. Install Docker and Docker-compose.
1. Copy the `v2ray-upstream-server` directory into the upstream server.
1. Run ```cat /proc/sys/kernel/random/uuid``` to generate a UUID.
1. Replace `<UPSTREAM-UUID>` in `config/config.json` with the generated UUID.
1. Run `docker-compose up -d`.

#### Setup Bridge Server

1. Install Docker and Docker-compose.
1. Copy the `v2ray-bridge-server` directory into the bridge server.
1. Replace the following variables in `config/config.json` with appropriate values.
    * `<SHADOWSOCKS-PASSWORD>`: A password for Shadowsocks users like `FR33DoM`.
    * `<BRIDGE-UUID>`: A new UUID for bridge server (Run ```cat /proc/sys/kernel/random/uuid```).
    * `<UPSTREAM-IP>`: The upstream server IP address (like `13.13.13.13`).
    * `<UPSTREAM-UUID>`: The upstream server UUID from previous step.
1. Run `docker-compose up -d`.
1. Run `./clients.py` to generate client configurations and links.

### Setup V2Ray Using a CDN Service

In this solution, you need one server (upstream) and a domain/subdomain added to a CDN service.

* Upstream Server: A server that has free access to the Internet.
* CDN Service: A Content delivery network like [Cloudflare](//cloudflare.com) or [ArvanCloud](//arvancloud.ir).

```
(Client) <-> [ CDN Service ] <-> [ Upstream Server ] <-> (Internet)
```


This solution provides VMESS over Websockets + TLS + CDN.
[Read more...](https://guide.v2fly.org/en_US/advanced/wss_and_web.html)

Follow these steps:

1. In your CDN, create an `A` record pointing to your server IP with the proxy option turned off.
1. Install Docker and Docker-compose on your server.
1. Copy the `v2ray-cdn-ready` directory into the server.
1. Run ```cat /proc/sys/kernel/random/uuid``` to generate a UUID.
1. Replace `<UPSTREAM-UUID>` in `v2ray/config/config.json` with the generated UUID.
1. Replace `<EXAMPLE.COM>` in `caddy/Caddyfile` with your domain/subdoamin.
1. Run `docker-compose up -d`.
1. Visit your domain/subdomain in your web browser. Wait until the homepage is loaded.
1. In your CDN, turn the proxy option on for the record.
1. Run `./vmess.py` to generate client configuration (link).

### Setup Client Applications

#### VMESS Protocol

This is the list of recommended applications to use the VMESS protocol:

* [Nekoray](https://github.com/MatsuriDayo/nekoray/releases) for macOS, Windows, and Linux
* [Qv2ray](https://qv2ray.net) for macOS, Windows, and Linux
* [V2RayX](https://github.com/Cenmrev/V2RayX/releases) for macOS
* [ShadowLink](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518) for iOS
* [v2rayNG](https://github.com/2dust/v2rayNG) for Android

#### Shadowsocks Protocol

This is the list of recommended applications to use the Shadowsocks protocol:

* [Outline](https://getoutline.org/get-started/#step-3) for macOS, Windows, and Linux
* [Shadowsocks](https://github.com/shadowsocks/ShadowsocksX-NG/releases) for macOS
* [Shadowsocks](https://github.com/shadowsocks/shadowsocks-libev) for Linux
* [Shadowsocks](https://github.com/shadowsocks/shadowsocks-windows/releases) for Windows
* [Shadowsocks](https://github.com/shadowsocks/shadowsocks-android/releases) for Android
* [ShadowLink](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518) for iOS

#### HTTP/HTTPS & SOCKS Protocols

Moved here: [HTTP_SOCKS.md](HTTP_SOCKS.md)

### Tips

* Some hostings might ban your proxy traffic. Use an appropriate hosting.
* Some Internet providers might ban your proxy traffic. Changin AlterID could be helpful.
  See [#57](https://github.com/miladrahimi/v2ray-docker-compose/issues/57).

### Docker images

By default, this repository uses the GitHub registry.
You can modify the Docker-compose file to use Docker Hub.

* GitHub:
  * Image: ```ghcr.io/getimages/v2fly-core:v4.45.2```
  * URL: https://github.com/orgs/getimages/packages/container/package/v2fly-core
  * Digest: `sha256:289fc9451f21a265f95615e29f05ea23bc32026db152863eee317738813521d7`
* Docker Hub:
  * Image: ```v2fly/v2fly-core:v4.45.2```
  * URL: https://hub.docker.com/r/v2fly/v2fly-core/tags
  * Digest: `sha256:289fc9451f21a265f95615e29f05ea23bc32026db152863eee317738813521d7`

## More

* [Docker Compose files to run an Outline bridge server](https://github.com/miladrahimi/outline-bridge-server)
* [V2Ray Config Examples](https://github.com/xesina/v2ray-config-examples)
* [Setup V2Ray servers using Ansible](https://github.com/ohmydevops/v2ray-ansible)
* [Read more about V2Fly and V2Ray](https://www.v2fly.org)
* [Read more about V2Ray configurations](https://guide.v2fly.org)
