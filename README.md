# Torric bootstrap file

`bootstrap.json` tells the Torric app where to find the network when the
servers it shipped with have moved or are unreachable: API endpoints,
signaling trackers, public WebTorrent trackers, DHT bootstrap nodes, and a
few public keys.

The same file is served from several places, and the app tries all of them:

- https://prastavna.github.io/torric-bootstrap/bootstrap.json (GitHub Pages)
- https://cdn.jsdelivr.net/gh/Prastavna/torric-bootstrap@main/bootstrap.json (jsDelivr)
- https://torric.app/bootstrap.json (Cloudflare)

## Trust

Hosting the file confers no authority over it. The file is signed with the
Torric **project key**, which is kept offline, and the app accepts it only if
the signature checks out against the public key built into the app:

```
joqo3nwWjlUuMtyw3X58KywkBlVaZXCKqMXiKDsTVQU
```

(Ed25519, base64url.) The signature (`sig`) covers the UTF-8 bytes of

```
torric/bootstrap/v1\n<canonical JSON of every field except sig>
```

where the canonical JSON has its object keys sorted, no whitespace, and only
integers as numbers. `seq` only goes up: an app keeps the newest file it has
verified and ignores any file with a lower or equal `seq`, so an old copy
can't roll it back.

Anyone can mirror this file, and a mirror can't change what it says. To
check a copy, verify the signature as described above.

## Changes

This repository is published by the maintainers' tooling. Don't edit
`bootstrap.json` by hand: any change breaks the signature, and apps will
refuse the file.
