# MEMO command list

## sbt tgz instead of ZIP

```bash
sbt universal:package-zip-tarball
```

## Play new app

```bash
sbt new playframework/play-scala-seed.g8
```

## return inserted ID on slick3

```scala
Items returning Items.map(_.id) += item
```

## Get current directory on bash

```bash
script_dir=$(cd $(dirname $(readlink $0 || echo $0));pwd -P)
```

## git

### make branch

```git
git branch develop
```

### change branch

```git
git checkout develop
```

### undo add

```git
git rm --cached [file]
```

### delete local branch

ex: use_ftp branch

```git
git branch -d use_ftp
```

### delete remote branch

```git
git push origin :use_ftp
```

### add tag

```git
git tag v1.0
```

### push tag

```git
git push origin v1.0
```

### modify commit log

```git
git commit --amend
```

## Systemd list auto starting units

```bash
systemctl list-unit-files|grep enabled
```


## Sphinx

### make a link to another rst file.

```python
 :doc:`filename`
```
