<div id="top" align="center">

<h1>Next SemVers</h1>

<p>GitHub Action that output the next version for major, minor, and patch version based on the given semver version.</p>

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![WyriHaximus](https://img.shields.io/badge/fork_from-WyriHaximus-black?logo=github)](https://github.com/WyriHaximus/github-action-next-semvers)

![GitHub Created At](https://img.shields.io/github/created-at/Zheng-Bote/repo-template)

</div>

## Brief

Github Action and Workflow to get previous tag / version and next (SemVer) version (See Example 1)

<hr>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Table of Contents**

-   [Description](#description)
-   [Options](#options)
    -   [version](#version)
    -   [strict](#strict)
-   [Output](#output)
-   [Examples](#examples)
    -   [Example 1](#example-1)
    -   [Example 2](#example-2)
-   [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

<hr>

## Description

![GHA](https://img.shields.io/badge/Github-Action-black?logo=githubactions)
![Node](https://img.shields.io/badge/Node-20-blue?logo=tsnode)

-   GitHub Action that output the next version for major, minor, and patch version based on the given semver version.

-   Github Action and Workflow to get previous tag / next version based on parameter Major|Minor|Patch. See Example 1

![Example output showing this action in action](images/output.png)

<p align="right">(<a href="#top">back to top</a>)</p>

## Status

![Status](https://img.shields.io/badge/Status-works-green)
![GitHub Release Date](https://img.shields.io/github/release-date/Zheng-Bote/gha-next-semvers)

[![Repo - create Repo-Tree in README.md](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-create_tree_readme.yml/badge.svg)](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-create_tree_readme.yml)
[![Repo - add Actions In/Out to README](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-actions_docu.yml/badge.svg)](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-actions_docu.yml)
[![Repo - create TOC of README](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-create_doctoc.yml/badge.svg)](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo-create_doctoc.yml)

<p align="right">(<a href="#top">back to top</a>)</p>

## Options

This action supports the following options.

### version

The version we want to have the next versions for.

-   _Required_: `Yes`
-   _Type_: `string`
-   _Example_: `v1.2.3` or `1.2.3`

### strict

Strict version validation, when turned off, the version is suffixed with `.0` until it contains 3 x `.`.

-   _Required_: `No`
-   _Type_: `string`
-   _Example_: `true` or `false`

## Output

This action output 6 slightly different outputs. A new major, minor, and patch version and a variant of those prefixed
with a `v`. For example when you input `1.2.3` it will give you the following outputs:

-   `major`: `2.0.0`
-   `minor`: `1.3.0`
-   `patch`: `1.2.4`
-   `v_major`: `v2.0.0`
-   `v_minor`: `v1.3.0`
-   `v_patch`: `v1.2.4`

In addition, if your input contains an indicator that it is a pre-release (e.g., `1.2.3-alpha`), the output for `patch` version changes accordingly (while the behaviour for `major` and `minor` are not affected and work as usual):

-   `major`: `2.0.0`
-   `minor`: `1.3.0`
-   `patch`: `1.2.3`
-   `v_major`: `v2.0.0`
-   `v_minor`: `v1.3.0`
-   `v_patch`: `v1.2.3`

<p align="right">(<a href="#top">back to top</a>)</p>

## Examples

### Example 1

```Shell
.github/workflows/repo-get_next_version.yml
```

[![call wf get next SemVer](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo_call_repo-get_next_version.yml/badge.svg)](https://github.com/Zheng-Bote/gha-next-semvers/actions/workflows/repo_call_repo-get_next_version.yml)

```yaml
name: call sub workflow - get next SemVer

on:
    workflow_dispatch:

jobs:
    job1:
        name: "Call other workflow"
        uses: Zheng-Bote/gha-next-semvers/.github/workflows/repo-get_next_version.yml@master
        with:
            Major: false
            Minor: true
            Patch: false

    job2:
        needs: job1
        runs-on: ubuntu-latest
        steps:
            - name: "Print my_var"
              run: |
                  echo "prevtag: ${{needs.job1.outputs.prevtag}}"
                  echo "vsemver: ${{needs.job1.outputs.vsemver}}"
                  echo "semver: ${{needs.job1.outputs.semver}}"
```

### Example 2

The following example works together with the [`WyriHaximus/github-action-get-previous-tag`](https://github.com/marketplace/actions/get-latest-tag)
and [`WyriHaximus/github-action-create-milestone`](https://github.com/marketplace/actions/create-milestone) actions.
Where it uses the output from that action to supply a set of versions for the next action, which creates a new
milestone. (This snippet has been taken from the automatic code generation of [`wyrihaximus/fake-php-version`](https://github.com/wyrihaximus/php-fake-php-version/).)

```yaml
name: Generate
jobs:
    generate:
        steps:
            - uses: actions/checkout@v1
            - name: "Get Previous tag"
              id: previoustag
              uses: "WyriHaximus/github-action-get-previous-tag@v1"
              env:
                  GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
            - name: "Get next minor version"
              id: semvers
              uses: "WyriHaximus/github-action-next-semvers@v1"
              with:
                  version: ${{ steps.previoustag.outputs.tag }}
            - name: "Create new milestone"
              id: createmilestone
              uses: "WyriHaximus/github-action-create-milestone@v1"
              with:
                  title: ${{ steps.semvers.outputs.patch }}
              env:
                  GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
```

# Parameters

<!-- only for actions repo -->

## Inputs

<!-- AUTO-DOC-INPUT:START - Do not remove or modify this section -->

|                         INPUT                         |  TYPE  | REQUIRED | DEFAULT  |                                                      DESCRIPTION                                                      |
|-------------------------------------------------------|--------|----------|----------|-----------------------------------------------------------------------------------------------------------------------|
|  <a name="input_strict"></a>[strict](#input_strict)   | string |  false   | `"true"` | Strict version validation, when turned <br>off, the version is suffixed <br>with `.0` until it contains <br>3 x `.`.  |
| <a name="input_version"></a>[version](#input_version) | string |   true   |          |                                                   A SemVer version                                                    |

<!-- AUTO-DOC-INPUT:END --> -->

## Outputs

<!-- AUTO-DOC-OUTPUT:START - Do not remove or modify this section -->

|                         OUTPUT                          |  TYPE  |              DESCRIPTION              |
|---------------------------------------------------------|--------|---------------------------------------|
|    <a name="output_major"></a>[major](#output_major)    | string |          Next Major Version           |
|    <a name="output_minor"></a>[minor](#output_minor)    | string |          Next Minor Version           |
|    <a name="output_patch"></a>[patch](#output_patch)    | string |          Next Patch Version           |
| <a name="output_v_major"></a>[v_major](#output_v_major) | string | Next Major Version (prefixed with v)  |
| <a name="output_v_minor"></a>[v_minor](#output_v_minor) | string | Next Minor Version (prefixed with v)  |
| <a name="output_v_patch"></a>[v_patch](#output_v_patch) | string | Next Patch Version (prefixed with v)  |

<!-- AUTO-DOC-OUTPUT:END --> -->

<p align="right">(<a href="#top">back to top</a>)</p>

## folder structure

<!-- readme-tree start -->
```
.
├── .editorconfig
├── .gitattributes
├── .github
│   ├── CODEOWNERS
│   ├── FUNDING.yml
│   ├── boring-cyborg.yml
│   ├── dependabot.yml
│   ├── settings.yml
│   └── workflows
│       ├── ci.yml
│       ├── craft-release.yaml
│       ├── repo-actions_docu.yml
│       ├── repo-create_toc.yml
│       ├── repo-create_tree_readme.yml
│       ├── repo-get_next_version.yml
│       ├── repo_call_repo-get_next_version.yml
│       └── set-milestone-on-pr.yaml
├── .gitignore
├── .php_cs
├── .scrutinizer.yml
├── CONTRIBUTING.md
├── Dockerfile
├── Dockerfile-build
├── LICENSE
├── Makefile
├── README.md
├── action.yml
├── composer-require-checker.json
├── composer.json
├── composer.lock
├── images
│   └── output.png
├── infection.json.dist
├── next.php
├── phpcs.xml.dist
├── phpstan.neon
├── phpunit.xml.dist
├── psalm.xml
├── src
│   └── Next.php
├── tests
│   └── NextTest.php
└── tree.bak

5 directories, 38 files
```
<!-- readme-tree end -->

<p align="right">(<a href="#top">back to top</a>)</p>

## License

Copyright 2019 [Cees-Jan Kiewiet](http://wyrihaximus.net/)

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

<p align="right">(<a href="#top">back to top</a>)</p>

### Code Contributors

![Contributors](https://img.shields.io/github/contributors/Zheng-Bote/gha-next-semvers?color=dark-green)

[![Zheng Robert](https://img.shields.io/badge/Github-Zheng_Robert-black?logo=github)](https://www.github.com/Zheng-Bote)[Zheng-Bote](https://www.github.com/Zheng-Bote)

<hr>

:vulcan_salute:

<p align="right">(<a href="#top">back to top</a>)</p>
