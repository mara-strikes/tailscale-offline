# tailscale-offline

Official Tailscale 1.104.5 static binaries, for fleet agents that cannot
reach tailscale.com (and whose policy declines anonymous file hosts).

These are byte-for-byte Tailscale's official release builds, as published at
https://pkgs.tailscale.com/stable/. Nothing here is modified or third-party.

## Files (release v1.104.5)

- `tailscale_1.104.5_amd64.tgz` — static binaries for x86_64
- `tailscale_1.104.5_arm64.tgz` — static binaries for aarch64 / ARM64

Each tarball holds the `tailscale` + `tailscaled` binaries and a `systemd/`
unit set (`tailscaled.service` etc.).

## Verify

```
sha256sum tailscale_1.104.5_<arch>.tgz
```

Expected:

```
720cbbccbfc03d8b55df72afc19e5a72e31b0f65934700aa6fa38074d4ad7bf7  tailscale_1.104.5_amd64.tgz
e900b0e6e25023a1e504b2c237ce2a32ea0535e006f8c1a1097a490886493b6c  tailscale_1.104.5_arm64.tgz
```

Tailscale's published checksums:
https://pkgs.tailscale.com/stable/tailscale_1.104.5_amd64.tgz.sha256
https://pkgs.tailscale.com/stable/tailscale_1.104.5_arm64.tgz.sha256

## Install (headless Ubuntu, root)

```
tar xzf tailscale_1.104.5_<arch>.tgz
cd tailscale_1.104.5_<arch>
sudo install -m 0755 tailscale tailscaled /usr/local/bin/
sudo cp systemd/tailscaled.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable --now tailscaled
sudo tailscale up
```

`tailscale up` prints a login URL; open it in a browser and sign in with the
fleet account to approve the device. The tailnet admin then tags the device
`tag:agent` so it can SSH to the fleet servers.
