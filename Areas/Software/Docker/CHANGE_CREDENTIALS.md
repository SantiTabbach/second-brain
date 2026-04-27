---
tags: [areas, software, docker]
author: Santi Tabbach
created: 2026-04-27
modified: 2026-04-27
---

```bash
sudo nano ~/.docker/config.json
```

```txt
{
        "auths": {
                "https://index.docker.io/v1/": {
                        "auth": "c3luYXB0YWh1YjpBTjNGNmZLTUJGWkRCcFNYNnZQMw=="
                }
        },
        "credsStore": "desktop", # <---- Remove this in order to allow bash login
        "currentContext": "desktop-linux",
        "plugins": {
                "debug": {
                        "hooks": "exec"
                },
                "scout": {
                        "hooks": "pull,buildx build"
                }
        },
        "features": {
                "hooks": "true"
        }
}
```