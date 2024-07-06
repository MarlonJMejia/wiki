---
description: 
date: 2024-07-05T21:12:23
tags: 
editor: markdown
dateCreated: 2024-07-05T21:12:23
---

[Unofficial Strict Mode](http://redsymbol.net/articles/unofficial-bash-strict-mode/)

??? info
    I call this the unofficial bash strict mode. This causes bash to behave in a way that makes many classes of subtle bugs impossible. You'll spend much less time debugging, and also avoid having unexpected complications in production.

    There is a short-term downside: these settings make certain common bash idioms harder to work with. Most have simple workarounds.

```bash
#!/bin/bash
set -euo pipefail
IFS=$'\n\t'
```