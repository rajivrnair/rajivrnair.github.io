---
title: Simple git hook to enforce commit mesage
tags: [git hook]
---

### Setting up Simple git hook to enforce a commit message

If you're working on a project with other people and want to have a consistent commit log,
here's a simple git commit hook to enforce a particular format.

I prefer to use the format `#<issue number> | <commit message>`

```bash

# In your repo...
$> vi .git/hooks/commit-msg
#!/bin/sh

test "" != "$(grep -Ei '^#[0-9]+\s?\|' "$1")" || {
    echo >&2 Aiyyo! Commit message needs to be in the form '#<issue number> | <text message>'.
    exit 1
}
```