name: Generate Profile Stats

on:
  schedule:
    - cron: "0 0 * * *"   # runs once a day at midnight UTC
  workflow_dispatch:        # lets you trigger it manually from the Actions tab

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Generate stats images
        uses: jstrieb/github-stats@master
        with:
          username: Jude-00-05
          exclude_repos: ""
          exclude_langs: ""
          generate_repo_list: false

      - name: Commit generated files
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add generated/*.svg
          git commit -m "Update generated stats" || echo "No changes to commit"
          git push
