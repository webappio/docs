# SKIP REMAINING IF

`SKIP REMAINING IF [KEY=VALUE]`

The `SKIP REMAINING IF` instruction will cause remaining instructions in the Layerfile to be skipped if the condition is evaluated to true.

Multiple `SKIP REMAINING IF` instructions may be declared in one Layerfile.

Conditions may use any variable from [`BUILD ENV`](/docs/layerfile-reference/build-env).

Conditions may use `AND` to group statements using logical AND.

Conditions may use `!=` to evaluate statements are not true.

### Examples

- Use `SKIP REMAINING IF GIT_BRANCH!=master` to skip execution on any branch that is not master.
- Use `SKIP REMAINING IF GIT_BRANCH!=master AND REPOSITORY_NAME !=~ "web"` would skip remaining actions if the branch is not master, and the repository name is not "web"
- Use `SKIP REMAINING IF GIT_COMMIT_TITLE =~ "\[skip tests\]"` would skip remaining actions if the commit title contained "\[skip tests]"
- Use `SKIP REMAINING IF GIT_BRANCH!=~^(master|dev)$` would skip remaining actions if the branch is anything besides `master` or `dev`

<br />