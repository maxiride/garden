---
title: Switched to Kiri instead of Docker Desktop
author: Federico D'Eredità
pubDatetime: 2026-11-23
slug: kiri-docker-desktop-alternative
featured: false
draft: false
tags:
  - docker
  - tools
  - rust
description:
  I've used Docker Desktop since it existed, and moved to Kiri with zero friction. Lighter, faster, open source, and a much smaller WSL footprint on Windows.
---

[Kiri](https://github.com/libracoder/kiri) is a container runtime written in Rust, pitched directly against Docker Desktop: same `docker` CLI, same Compose and buildx, but small and fast where Docker Desktop is slow to start and heavy when idle. The name comes from the Igbo word *ntakịrị*, "small", which tells you what the project is optimizing for.

I've used Docker and Docker Desktop since they first came out. Switching was easy to install and the migration was frictionless, nothing to relearn, nothing broken.

## How it's built

Kiri runs your containers inside one small Linux VM on macOS and Windows, or straight on the host kernel on Linux, and talks the Docker Engine API, which is why your existing tooling just works against it. Under the hood the guest is a custom 10 MB Linux kernel, and PID 1 in that VM is a Rust agent under 1 MB. Starting a container is a single `clone3` call into namespaces and a cgroup, there's no `runc`, no shim per container, and no `dockerd`. That's the structural reason it's lighter: Docker Desktop's stack has more layers doing the same job.

## Features Docker Desktop doesn't have

A few things stood out as not just "faster Docker Desktop" but genuinely absent from Docker Desktop:

- **Local domains with HTTPS**: every container gets `http://<name>.kiri.local` for free, with TLS from Kiri's own certificate authority, no manual proxy or cert setup.
- **Linux machines**: `kiri machine create` spins up a full Linux VM with your user, `sudo` and home folder mounted in, not just containers.
- **A built-in, free Ngrok-style tunnel**: `kiri share <port>` puts a local port or container on the public internet without a separate paid service.
- **One-command import**: `kiri import` migrates images, volumes and containers over from OrbStack or Docker Desktop, which is presumably how most people's first run goes.
- **It's actually open source**: Apache-2.0, versus Docker Desktop's closed source and paid tiers for larger companies.

For local development with Docker, I find Kiri just more adequate.
