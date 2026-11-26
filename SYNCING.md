# Syncing this fork with upstream

This fork (`gerrypedraza/sink`) deploys to Cloudflare Workers from the `master` branch. Every push to `master` goes live, so upstream changes reach `master` only through a reviewed pull request.

- `master`: what's deployed. It's pinned to an upstream release (it started at v0.3.1, `bafa4e1`) and protected: a pull request is required, the rule applies to admins too, and force pushes and branch deletion are blocked.
- `upstream-sync`: a staging branch that tracks upstream `miantiao-me/Sink`. Upstream updates land here first.

**Don't use GitHub's "Sync fork" button.** It updates `master` directly.

## Pull in upstream updates

```sh
git remote add upstream https://github.com/miantiao-me/Sink.git   # first time only
git fetch upstream --tags
git checkout upstream-sync
git merge --ff-only upstream/master      # or a release tag, e.g. git merge --ff-only v0.3.2
git push origin upstream-sync
```

Then open a pull request from `upstream-sync` into `master` (or from a branch cut at a specific release tag), review the diff and release notes, and merge it yourself. Cloudflare deploys after the merge.

To move to a specific release only:

```sh
git checkout -b update-v0.3.2 v0.3.2
git merge master                         # keeps this fork's own commits, like this file
git push origin update-v0.3.2            # then open a PR into master
```

## Hosting / teardown

This fork is served by Cloudflare Workers at go.gerrypedraza.com and go.esail.work. If this repo or its Workers project is ever deleted or renamed, remove those DNS records and routes in Cloudflare at the same time.
