# hello

Testing out fluxcd.

## Develop Workflow
1. Modify apps/hello (including prep for prod)
1. Commit, push, tag deploy/hello/v[something]
1. Modify live/hello/path with new tag on relevant namespace(s)
1. kubectl apply live/hello/path
1. Commit, push to main

## Hotfix workflow
1. Check out tag, e.g. deploy/hello/v2
2. Branch from tag, e.g. hotfix/hello/v2
3. Make changes, commit, push branch, tag it
4. Modify live/hello/path with new tag on relevant namespace(s)
5. kubectl apply live/hello/path
6. Merge back hotfix/hello/v2 to main (See problem 1)

## Possible workflow 1
Use branches instead of tags and flux seems to run apply with certain intervals.

## Possible workflow 2
1. Modify apps/hello (including prep for prod)
1. Commit, push, tag deploy/hello/v[something]
1. Modify live/hello/path with new tag on relevant namespace(s)
1. Create PR, github actions run kubectl diff with clever script
1. Merge PR, github actions run kubectl apply with clever script / or flux can possibly handle it itself if it can monitor the mainfests in the live directory

## Factor out cluster specific config?
Store application config in blob storage and use clever script to kubectl apply with envsubst (requires cluster specific config to be stored in live-directory?).

## Problem 1
Given:
staging: deploy/v2
production: deploy/v2
Then:
1. deploy staging: deploy/v3
1. hotfix production: hotfix/v2/1
1. merge hotfix/v2/1 to main
1. deploy production deploy/v3 (hotfix is gone)

Might be solved if keeping namespace/cluster configurations on the main branch in the live-directory...?
