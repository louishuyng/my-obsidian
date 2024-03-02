---
tags: Docker, Devops, Security
---

## Add additional privileges

```bash
docker run --cap-add MAC_ADMIN ubuntu
```

## Remove privileges

```bash
docker run --cap-drop KILL ubuntu
```

## Enable all privileges

```
docker run --privileged ubuntu
```