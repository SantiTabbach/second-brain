---
tags: [areas, software, n8n]
author: Santi Tabbach
created: 2026-04-27
modified: 2026-04-27
---

### Run n8n locally

```bash
sudo docker run -it --rm \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```