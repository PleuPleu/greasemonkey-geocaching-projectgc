# Working in this repository

This repository contains a single userscript, `greasemonkey-geocaching-projectgc.user.js`.
Everything users install comes from that one file on `master`.

## Versioning

The `@version` line in the userscript header is what triggers updates for installed
users, via the `@updateURL`/`@downloadURL` metadata pointing at the raw file on `master`.

Bump `@version` at most once per change set. If several edits are made before
committing, they share a single bump — do not bump again for each edit.

## Releases

Releases are **not** automatic. Whenever a commit that bumps `@version` is pushed,
create the matching tag and GitHub release in the same sitting.

Create tags locally rather than through GitHub's web UI, so that the tag and the
commit it points at are verified together.

```sh
git fetch --tags origin                    # a previous release may have been tagged elsewhere
git tag vX.Y.Z <version-bump-commit>       # lightweight, matching the existing tags
git push origin vX.Y.Z
gh release create vX.Y.Z --title "vX.Y.Z" --target master \
    --notes "<end-user-focused summary>" --generate-notes
```

Conventions to keep consistent with existing releases:

* Tags are **lightweight** (plain pointers to a commit), not annotated.
* The tag name is `vX.Y.Z`, matching `@version` exactly, and points at the commit
  that bumped `@version`.
* The release is named `vX.Y.Z`, targets `master`, and is neither a draft nor a
  prerelease.

### Writing release notes

Release notes are read by geocachers, not developers. Keep them short and describe
the change in terms of what a user notices — a single plain sentence is usually
enough. `--generate-notes` appends a **Full Changelog** link, and that link is where
anyone wanting the technical detail should go, so there is no need to explain
implementation in the note itself.

Prefer:

> Fixes dependency issues with GreasyFork.

Over:

> Fix installation failing with a 403 by requiring libraries from
> update.greasyfork.org, and load jQuery over HTTPS.

Commit messages are the right place for the technical reasoning; release notes are not.
