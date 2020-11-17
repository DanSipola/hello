# hello

Testing out fluxcd.

## Develop Workflow
1. Modify deployment config apps/hello/
2. Commit and push
3. Changes are applied on staging

# Release to prod workflow (Tag option)
1. Tag the commit to be deployed, e.g. deployment/hello/1
2. Push the tag
3. Modify live/hello/clusters/ with new tag and push to main(production is updated)

### Hotfix workflow
1. Create a release branch from the tag currently deployed, e.g. hotfix/hello/1 and push it
2. Commit and push to the hotfix branch
3. Tag the commit, e.g. hotfix/hello/1-1
4. Set the new tag in Modify live/hello/clusters/ and push to main
5. Merge hotfix/hello/1 to main

## Release to prod workflow (Branch tracking option)
1. Branch from the commit to be deployed, e.g. release/hello/1
2. Push the branch
3. Modify live/hello/clusters/ to track the new branch

### Hotfix workflow
1. Make the changes and push to the currently released branch, e.g. release/hello/1
2. Changes are deployed
3. Merge release/hello/1 to main

## Problem if staging is deployed from tag
Given:
staging: deploy/v2
production: deploy/v2
Then:
1. deploy staging: deploy/v3
1. hotfix production: hotfix/v2/1
1. merge hotfix/v2/1 to main
1. deploy production deploy/v3 (hotfix is gone)
