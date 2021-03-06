# Labeler [![Build Status](https://travis-ci.org/tonglil/labeler.svg?branch=master)](https://travis-ci.org/tonglil/labeler)

![logo](http://i.imgur.com/5wOQl2m.png)

Label management (create/rename/update/delete) on Github as code.

- [x] Using GitHub?
- [x] Want to commit/copy/share your label configuration?
- [ ] Use `labeler`!

For FOSS maintainers, enable your users to submit PRs and improve the process/label system!
- [Clean up][adobe] your labels.
- Move labels out of the [same][iconic] [flat][certbot] [space][ghost].
- Enforce a label color scheme that is not [meaningless][node] nor [confusing][babel] to view.

Inspired by [infrastructure as code][iac] tools like [Terraform][terraform] and organized label systems in projects like these:
- https://github.com/kubernetes/kubernetes/labels
- https://github.com/coreos/etcd/labels
- https://github.com/coreos/rkt/labels
- https://github.com/spf13/hugo/labels
- https://github.com/docker/docker/labels

[adobe]: https://github.com/adobe/brackets/labels
[iconic]: https://github.com/driftyco/ionic/labels
[certbot]: https://github.com/certbot/certbot/labels
[ghost]: https://github.com/TryGhost/Ghost/labels
[node]: https://github.com/nodejs/node/labels
[babel]: https://github.com/babel/babel/labels

[iac]: http://martinfowler.com/bliki/InfrastructureAsCode.html
[terraform]: https://github.com/hashicorp/terraform

## Installation

Get binaries for OS X / Linux / Windows from the latest [release].

Or use `go get`:

```
go get github.com/tonglil/labeler
```

[release]: https://github.com/tonglil/labeler/releases

## Usage

Read a [GitHub token][tokens] from the environment and apply the label configuration:
```
export GITHUB_TOKEN=xxx
labeler -v 5 labels.yaml
```

> - The token for public repos need the `public_repo` scope.
> - The token for private repos need the `repo` scope.

Where `labels.yaml` is like:
```yml
repo: owner/name
labels:
  - name: bug
    color: fc2929
  - name: help wanted
    color: 000000
  - name: fix
    color: cccccc
    from: wontfix
  - name: notes
    color: fbca04
```

Which when run against a "new" repo created on GitHub, will:
- Rename `wontfix` to `fix` with color `ffffff` to `ffffff`
- Update `help wanted` with color `159818` to `000000`
- Create `notes` with color `fbca04`
- Delete `duplicate` with color `cccccc`
- Delete `enhancement` with color `84b6eb`
- Delete `invalid` with color `e6e6e6`
- Delete `question` with color `cc317c`

When run again, rename changes will not be run because the label already exists.
In this manner, this tool is idempotent.

[tokens]: https://github.com/settings/tokens

## Usage options

```
usage: labeler [<options>] <file.yaml>
```

Notable options:
- `-dry-run` - show what would happen (default false)
- `-v <0 to 9>` - log level for V logs (default 0)
- `-endpoint <https://url/>` - use a different GithHub API endpoint [overrides GITHUB_API environment variable] (default "https://api.github.com/")
- `-repo <owner/name>` - use a different repository (default "from file")
- `-token <string>` - use a different GithHub token [overrides GITHUB_TOKEN environment variable]
- `-version` - show version

Get all options by running:
```
labeler -h
```

## Development

[`glide`][glide] is used to manage vendor dependencies.

Roadmap:
- Scan existing labels from a repository and save it to a file.
- Refactor cli (https://github.com/tonglil/labeler/issues/5).
- Plan -> execute (aka always dry-run first).
- Automatically update file after renaming operations are complete.
- Organizational support (apply/only-add one config to all repos in an organization).

[glide]: https://github.com/Masterminds/glide

## Testing

**This could use your contribution!**
Help me create a runnable test suite.

## See also

- Rust: https://github.com/jimmycuadra/ghlabel
- Node: https://github.com/popomore/github-labels
- Node: https://github.com/repo-utils/org-labels
- PHP: https://gist.github.com/zot24/0cbbd3ee4b22123cb62a
