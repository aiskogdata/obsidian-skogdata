```sh
 git branch | grep -Ev "(^\*|^\+|master|main|dev" | xargs --no-run-if-empty git branch -d
```

Force delete
```sh
 git branch | grep -Ev "(^\*|^\+|master|main|dev)" | xargs --no-run-if-empty git branch -D
```