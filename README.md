# trident-github-tag-action

A Github Action to automatically bump and tag master, on merge, with the latest [SemVer](https://semver.org/) formatted version.

Given a version number `MAJOR.MINOR.PATCH-PREREL+BUILD`, increment the:
* **MAJOR** version when you make incompatible API changes,
* **MINOR** version when you add functionality in a backwards compatible manner, and
* **PATCH** version when you make backwards compatible bug fixes.
* **PREREL** *(optional)* pre-release metadata (e.g. `SNAPSHOT`).
* **BUILD** build metadata.

### Usage
Put version.properties file to project root.
```
MAJOR=1
MINOR=0
PATCH=0
PREREL=SNAPSHOT
```
Create github workflow file.
```
name: Bump version

on:
  push:
    branches:
    - master
    - hotfix/*

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: Bump version and push tag
      uses: iNomaD/github-tag-action@dev
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        WITH_V: false
        VERSION_FILE_PATH: ./version.properties

```

#### Options

**Environment Variables**

* **GITHUB_TOKEN** ***(required)*** - Required for permission to tag the repo.
* **WITH_V** *(optional)* - Tag version with `v` character. (default `false`)
* **VERSION_FILE_PATH** *(optional)* - Path to version file. (default `version.properties`)

#### Outputs

* **new_tag** - The value of the newly created tag.
* **tag** - The value of the latest tag after running this action.

> ***Note:*** This action creates a [lightweight tag](https://developer.github.com/v3/git/refs/#create-a-reference).

### Bumping

**Manual version Bumping:** Edit version file manually.

**Automatic build Bumping:** Every time a pull request is merged or a commit is pushed directly to `master` or `hotfix/*` branch, build number will be automatically bumped and a tag created.

> ***Note:*** This action **will not** bump the tag if the `HEAD` commit has already been tagged.

### Workflow

* Add this action to your repo
* Commit some changes
* Either push to master or open a PR
* On push (or merge) to `master`, the action will:
  * get latest tag for `MAJOR.MINOR.PATCH*`;
  * bump build number;
  * push tag to github.

### Credits

* [fsaintjacques/semver-tool](https://github.com/fsaintjacques/semver-tool)
* [Creating A Github Action to Tag Commits](https://itnext.io/creating-a-github-action-to-tag-commits-2722f1560dec)

