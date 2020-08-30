# Comment Command

When used as a step in a workflow, this GitHub Action parses the issue comment and returns the first word as the command and the second and third words as the arguments. The command should begin with `/`, and the issue comment should belong to a pull request, not an issue.

Works best for deployments using GitHub Actions.

## Example

```
/deploy production
```

Will output:

- command: deploy
- arguments: production

It only takes the first two words as the arguments. For example:

```
/deploy production canary test example one two
```

Will output:

- command: deploy
- arguments: production canary

## Usage

Make sure you have the following format in your workflow file:

```
on:
issue_comment:
  types:
  - created
jobs:
  [job_name]:
    runs-on: [your-environment]
    if: (startsWith(github.event.comment.body, '/') && github.event.issue.pull_request)
    steps:
      - name: Comment Command
        id: comment-command
        uses: lowply/comment-command@v1
```

In the following step, you can use the `command` and `arguments` outputs:

```
      - name: Checkout PR head
        uses: actions/checkout@v2
        if: contains(steps.comment-command.outputs.command, 'deploy')
        with:
          ref: [ref]
```

Visit https://github.com/lowply/comment-command for more info.
