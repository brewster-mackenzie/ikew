# Basic GitHub repository security for CI/CD

#git #ci-cd #security

-----

My minimum requirements to consider a GitHub respository to be safe 
to use for CI/CD in a production environment.

- a CODEOWNERS file to limit merges to a subset of users
- branch protection on `main` to force approval before merge 
- branch protection must not allow for self-review or approval override 
  - tick "Prevent self-review"
  - untick "Allow administrators to bypass configured protection rules"
- releases must be tagged with some kind of semantic versioning, e.g.
  - `X.Y.Z`
  - `appname-X.Y.Z`
  - `appname_lib-X.Y.Z`
- releases will be flagged "pre-release" until all non-production stage gates
have been satisfied
- releases will not be flagged as "pre-release" at the time that they are pushed
to a production environment
- any GitHub actions that target production environments will use a trigger that
requires approval from the reviewers


> [!tip]
> A note on semantic versioning
> While it might seem desirable to use prefixes in the release tags, 
> e.g. `appname-X.Y.Z` you will find that certain semver libraries do
> not parse this format correctly.  If possible, just use `X.Y.Z` without
> any decorative text.
