# Next SemVers

GitHub Action that output the next version for major, minor, and patch version based on the given semver version.



<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [brief](#brief)
- [Options](#options)
  - [version](#version)
  - [strict](#strict)
- [Output](#output)
- [Examples](#examples)
  - [Example 1](#example-1)
  - [Example 2](#example-2)
- [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



## brief

Github Action and Workflow to get previous tag / version and nest version (optional: based on parameter. See Example 1)


![Example output showing this action in action](images/output.png)

## Options

This action supports the following options.

### version

The version we want to have the next versions for.

* *Required*: `Yes`
* *Type*: `string`
* *Example*: `v1.2.3` or `1.2.3`

### strict

Strict version validation, when turned off, the version is suffixed with `.0` until it contains 3 x `.`.

* *Required*: `No`
* *Type*: `string`
* *Example*: `true` or `false`

## Output

This action output 6 slightly different outputs. A new major, minor, and patch version and a variant of those prefixed
with a `v`. For example when you input `1.2.3` it will give you the following outputs:

* `major`: `2.0.0`
* `minor`: `1.3.0`
* `patch`: `1.2.4`
* `v_major`: `v2.0.0`
* `v_minor`: `v1.3.0`
* `v_patch`: `v1.2.4`

In addition, if your input contains an indicator that it is a pre-release (e.g., `1.2.3-alpha`), the output for `patch` version changes accordingly (while the behaviour for `major` and `minor` are not affected and work as usual):

* `major`: `2.0.0`
* `minor`: `1.3.0`
* `patch`: `1.2.3`
* `v_major`: `v2.0.0`
* `v_minor`: `v1.3.0`
* `v_patch`: `v1.2.3`


## Examples

### Example 1

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
      - name: 'Get Previous tag'
        id: previoustag
        uses: "WyriHaximus/github-action-get-previous-tag@v1"
        env:
          GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
      - name: 'Get next minor version'
        id: semvers
        uses: "WyriHaximus/github-action-next-semvers@v1"
        with:
          version: ${{ steps.previoustag.outputs.tag }}
      - name: 'Create new milestone'
        id: createmilestone
        uses: "WyriHaximus/github-action-create-milestone@v1"
        with:
          title: ${{ steps.semvers.outputs.patch }}
        env:
          GITHUB_TOKEN: "${{ secrets.GITHUB_TOKEN }}"
```

## License ##

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
