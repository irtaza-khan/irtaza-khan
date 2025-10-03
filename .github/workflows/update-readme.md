name: Update README

on:
schedule: - cron: "0 0 \* \* \*" # runs every day at midnight UTC
workflow_dispatch: # allows manual trigger from Actions tab

jobs:
update-readme:
runs-on: ubuntu-latest
steps: - uses: actions/checkout@v4

      - name: Update GitHub Stats (force refresh)
        run: |
          # Add a cache-busting query parameter to force image refresh
          sed -i "s/cache_seconds=[0-9]*/cache_seconds=$RANDOM/" README.md

      - name: Commit and Push changes
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add README.md
          git commit -m "chore: auto-update README [skip ci]" || echo "No changes to commit"
          git push
