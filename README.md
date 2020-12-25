#File: .github/workflows/repo-sync.yml
name: sync-sazs34-scripts
on:
  schedule:
    - cron: '5 0,15 * * *'
  workflow_dispatch:
  watch:
    types: started
  repository_dispatch:
    types: sync-sazs34-scripts
jobs:
  repo-sync:
    env:
      PAT: ${{ secrets.PAT }} #此处PAT需要申请，教程详见：https://www.jianshu.com/p/bb82b3ad1d11
    runs-on: ubuntu-latest
    if: github.event.repository.owner.id == github.event.sender.id
    steps:
      - uses: actions/checkout@v2
        with:
          persist-credentials: false

      - name: sync sazs34-scripts
        uses: repo-sync/github-sync@v2
        if: env.PAT
        with:
          source_repo: "https://github.com/sazs34/MyActions.git"
          source_branch: "master"
          destination_branch: "main"
          github_token: ${{ secrets.PAT }}

