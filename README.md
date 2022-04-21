# heroku

`dentarg/heroku` is an [composite run steps action] that deploys your app to Heroku.

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
    runs-on: ubuntu-latest
    steps:
      - uses: dentarg/heroku@v1
        with:
          app: myapp
          key: ${{ secrets.HEROKU_API_KEY }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

[composite run steps action]: https://docs.github.com/en/free-pro-team@latest/actions/creating-actions/creating-a-composite-run-steps-action
