---
date: 2025-12-16
---
# Linux Commands Every Dev Should Know

> **Description**: A cheat sheet of core Linux command-line concepts for developers—finding things fast, moving around efficiently, viewing/parsing data, and more.

#Linux #commands

---

## 📋 **Table of Contents**

- [Search & Find](#search--find)
- [Navigation & Listing](#navigation--listing)
- [Viewing & Parsing Files](#viewing--parsing-files)
- [Containers](#containers)
- [System Info](#system-info)
- [Processes & Services](#processes--services)
- [Networking](#networking)
- [Monitoring & Diagnostics](#monitoring--diagnostics)
- [Disk & Storage](#disk--storage)
- [Help & Docs](#help--docs)
- [Validation](#validation)
- [Performance](#performance)
- [Git](#git)
- [GitHub CLI](#github-cli)
- [Resources](#)
---

## Search & Find

1. Search for the word `"auth"` in every file of the directory

```bash
rg -w "auth" .
rg -w "auth" /some/repo
```

2. Filter [[Docker#Docker Compose Commands|docker compose]] logs for errors (uses ripgrep)

```bash
docker compose logs -f | rg -i "error"
```

3. Search file names (regex by default)

```bash
fd "go\.mod|Cargo\.toml|pom\.xml" repos lab
```

- `|` acts as regex `or`.

4. Fuzzy finder combos

```bash
fd "go\.mod|Cargo\.toml|pom\.xml" repos lab | fzf
rg -w "auth" /some/repo | fzf
docker compose logs -f | fzf
```

## Navigation & Listing

5. Show directory architecture

```bash
tree -L 3
```

- `-L 3` limits to three directories deep.

6. Use multiple terminals in one window

```bash
tmux
```

7. Faster `cd` with frecency

```bash
z newproject
```

- Works after installing `z`/`zoxide` and building history.

8. `eza` is an enhanced `ls`

```bash
eza -lah
eza -l --icons
eza --tree -L 3
```

## Viewing & Parsing Files

9. Better `cat`

```bash
bat -n file.md
```

10. Parse JSON

```bash
jq '.items[] | .name' data.json
```

11. HTTP requests

```bash
curl -s https://example.com
curl -I https://example.com
```

- `-s` is silent mode; `-I` fetches headers only.

## Containers

12. Tail [[Docker#Docker Compose Commands|docker compose]] logs with filtering

```bash
docker compose logs -f | rg -i "error"
```

13. TUI for [[Docker]]

```bash
lazydocker
```

## System Info

14. System summary (CPU, GPU, etc.)

```bash
fastfetch
```

## Processes & Services

15. Service management (user scope example)

```bash
systemctl --user status gin-service
```

16. Modern process viewer

```bash
procs -a
```

## Networking

17. See what is listening on a port

```bash
lsof -nP -iTCP:8099 -sTCP:LISTEN
```

18. Socket statistics and listeners

```bash
ss -ltnp | rg ":8099|:15432|:16379"
```

- Use `sudo` if you need process names for all sockets.

## Monitoring & Diagnostics

19. Refresh a command repeatedly

```bash
watch -n 0.5 'ss -ltnp | rg ":8099|:15432|:16379"'
```

- Refreshes twice per second with `-n 0.5`.

## Disk & Storage

20. Interactive disk usage viewer

```bash
ncdu
```

## Help & Docs

21. Concise command examples

```bash
tldr rg
```

## Validation

22. Lint shell scripts

```bash
shellcheck deploy.sh
```

## Performance

23. Measure command performance (5 runs averaged)

```bash
perf stat -r 5 curl -s localhost:8099/health >/dev/null
```

- Requires `perf` permissions (often `sudo` or `perf_event_paranoid` adjustment).

## [[GIT]]

24. Search in repo

```bash
git grep "test" || true
```

25. Manage worktrees (stash alternative)

```bash
git worktree list
git worktree add ../feature feature-branch
```

26. Find the commit that introduced a bug

```bash
git bisect start
```

- Then: `git bisect bad`, `git bisect good <commit>`, test, and `git bisect reset` when done.

27. Prettier git diff

```bash
git diff | delta
```

28. TUI for git workflows

```bash
lazygit
```

## GitHub CLI

29. List issues

```bash
gh issue list
```

30. List pull requests

```bash
gh pr list
```

---

## 📚 Resources

- [33 Linux Commands EVERY Dev Should Know - ForrestKnight](https://www.youtube.com/watch?v=XhLMS47l8Bw&list=WL&index=1)
