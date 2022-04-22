# heroku

`dentarg/heroku` is an [composite run steps action] that deploys your app to Heroku, by `git push`.

The Heroku API key (`HEROKU_API_KEY`) needs to be saved as a [secret in GitHub Actions]. Use [`heroku authorizations`] to generate the API key, see this [Heroku help article] for more information.

If you pass `github-token`, the action will create [deployments] in your repository. When you use this, the workflow needs not to be triggered by the [`push` event] to work. See the example below.

The example workflow below deploys the app when the `CI` workflow ran succesfully against the default branch.

```yaml
name: Deploy to Heroku

on:
  workflow_run:
    workflows: [CI]
    types: [completed]

jobs:
  deploy:
    if: |
      github.event.workflow_run.conclusion == 'success' &&
      github.event.workflow_run.head_branch == github.event.repository.default_branch
    concurrency: myapp
    runs-on: ubuntu-latest
    steps:
      - uses: dentarg/heroku@v1
        with:
          app: myapp
          key: ${{ secrets.HEROKU_API_KEY }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

[composite run steps action]: https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/creating-a-composite-run-steps-action
[secret in GitHub Actions]: https://docs.github.com/en/actions/security-guides/encrypted-secrets
[`heroku authorizations`]: https://github.com/heroku/cli/blob/master/docs/authorizations.md
[Heroku help article]: https://help.heroku.com/PBGP6IDE/how-should-i-generate-an-api-key-that-allows-me-to-use-the-heroku-platform-api
[deployments]: https://docs.github.com/en/rest/deployments/deployments
[`push` event]: https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#push
