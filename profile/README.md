<p align="center"><img src="https://raw.githubusercontent.com/go-fileshare/brand/main/social/go-fileshare.png" alt="go-fileshare" width="640"></p>

<h1 align="center">go-fileshare</h1>
<p align="center">One disk image, served over SMB, NFS, WebDAV and SFTP — the same users, the same per-share access, from one configuration file.</p>
<p align="center">
  <img src="https://img.shields.io/badge/Go-1.26-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img src="https://img.shields.io/badge/license-BSD--3--Clause-0A6E96?style=flat-square">
  <img src="https://img.shields.io/badge/cgo-none-0079A8?style=flat-square">
  <a href="https://github.com/go-filesystems"><img src="https://img.shields.io/badge/drivers-go--filesystems-0079A8?style=flat-square"></a>
</p>

---

## What this is

A person wants to **share an image**. Which protocol carries it is a property
of the client at the other end: macOS and Windows reach for SMB, a Linux fleet
already has NFS, a browser or a phone has HTTP. Running four servers, each with
its own configuration file and its own idea of who "alice" is, is a way to get
three of them subtly wrong.

So this is one binary and one file. The images are [`go-filesystems`](https://github.com/go-filesystems) drivers —
FAT32, exFAT, ext4, NTFS, **UFS**, ISO 9660, SquashFS, HFS+ — opened once and
shared by every protocol behind one lock. That is every driver in the
organisation of the one shape; APFS, Btrfs, XFS and ZFS open a *disk* image and
pick a partition instead, which is a different question about what a share is.

## The protocols do not agree about who is asking

That is the whole difficulty, and it is a fact about the protocols rather than
a limitation here:

| | proves somebody by |
|---|---|
| **SMB** | NTLMv2 — the password never crosses the wire |
| **WebDAV** | HTTP Basic, or a **bearer token** an identity provider signed |
| **SFTP** | a public key, or an SSH certificate from an authority you trust |
| **NFSv3** | **nothing at all** — `AUTH_UNIX` is a claim the client makes about itself |

So **a share that names who may use it is not exported over NFS**. Not a
warning and not an option: a configuration that says "photos belongs to alice"
and a protocol that hands photos to whoever connects cannot both be honoured,
and quietly widening access is the worse of the two failures. `fileshare check`
prints the whole matrix — every share against every protocol, and every person
against every protocol — before anything is restarted.

## Repos

| | Repo | |
|---|---|---|
| <img src="https://raw.githubusercontent.com/go-fileshare/brand/main/avatar/go-fileshare-fileshare.png" width="36"> | [`fileshare`](https://github.com/go-fileshare/fileshare) | The server: one image, four protocols, one configuration. `go install github.com/go-fileshare/fileshare@latest` |

## Where the people come from

A `user` block is the whole directory for a household. A site whose people are
already in a database or in LDAP names them where they are, through
[`go-authn/directory`](https://github.com/go-authn/directory) — the same
`users` block an [`authnd`](https://github.com/go-authn/authnd) uses, so a site
describes its directory once.

## Links

- 📖 Docs — <https://go-fileshare.github.io/docs/>
- 🌐 Site — <https://go-fileshare.github.io/>
- 🎨 Brand assets — <https://github.com/go-fileshare/brand>

---

<p align="center"><sub>No cgo. Six architectures under qemu. Verified against clients this project did not write: Samba's <code>smbclient</code>, macOS's Finder and Windows' own redirector, OpenSSH's <code>sftp</code>, and <code>mount_nfs</code>.</sub></p>
