# Summary
Note - this is unmodified copy of https://github.com/go-yaml/yaml, branch v2, as of commit https://github.com/go-yaml/yaml/commit/770b8dae4cf00919e5eafffbd8d58186294b61b5 Date:   Tue Nov 5 08:55:36 2019 -0800. The original LICENSE applies.

# Problem
The repo is copied to due the current failures when trying to mirror
https://github.com/go-yaml/yaml in a different server such as git.xyzcompany.com, and using the following `replace` in `go.mod`:
```
replace gopkg.in/yaml.v2 => git.xyzcompany.com/mirror-github/go-yaml--yaml v2.2.2+incompatible
```

`go mod download` then fails:
```
$ go mod download
go: finding git.xyzcompany.com/mirror-github/pkg--errors v0.8.1+incompatible
go: finding git.xyzcompany.com/mirror-github/go-yaml--yaml v2.2.2+incompatible
git.xyzcompany.com/mirror-github/pkg--errors@v0.8.1+incompatible: invalid version: +incompatible suffix not allowed: major version v0 is compatible
git.xyzcompany.com/mirror-github/go-yaml--yaml@v2.2.2+incompatible: invalid version: +incompatible suffix not allowed: module contains a go.mod file, so semantic import versioning is required
```

However, an attempt to remove `+incompatible` from the version specification in the rewrite statement by changing it to `gopkg.in/yaml.v2 => git.xyzcompany.com/mirror-github/go-yaml--yaml v2.2.2` fails as well (and actually worse - now not only `go mod download` but also `go mod verify` and `go install` don't work because the path doesn't contain `v2`:
```
$ go mod verify
go: finding git.xyzcompany.com/mirror-github/go-yaml--yaml v2.2.2
go: errors parsing go.mod:
/Users/dmitris/dev/path/to/project/go.mod:63: replace git.xyzcompany.com/mirror-github/go-yaml--yaml: version "v2.2.2" invalid: module contains a go.mod file, so major version must be compatible: should be v0 or v1, not v2
```

# Solution
The following works - no error from `go mod download` and other `go` commands:

`replace gopkg.in/yaml.v2 => github.com/dmitris/go-yaml-v2 v0.2.5`


