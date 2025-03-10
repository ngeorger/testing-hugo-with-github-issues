---
title: "Titulo a publicar"
date: 2023-01-17T03:31:58Z
author: "ngeorger"
draft: false
type: post
tags: []
categories: null
image: null
---

### Autor

Juanito Pérez

### Categoria

Noticias

### image

![](https://raw.githubusercontent.com/sredevopsdev/elclaustro/main/static/images/wp-content/logo.png)


### Contenido

# EO

eeeeo

```yaml
name: Convert issues to markdown.
on:
  issues:
    types: [opened, edited]

    
  workflow_dispatch:
    
jobs:
  convert-issues-to-markdown:
    runs-on: ubuntu-latest
    permissions:
        contents: write
        pull-requests: write
        issues: read
        id-token: read
    
    name: Convert issues to markdown.
    steps:
      - name: checkout
        uses: actions/checkout@v3
        with:
          ref: main
          persist-credentials: true
          

      - name: Fetch issues and generate markdowns
        uses: sredevopsdev/hugo-with-github-issues@main
        id: hugo-with-github-issues
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          repo: 'hugo-with-github-issues'
          owner: 'sredevopsdev'
          skip-author: 'github-actions[bot]'
          use-issue-seperator: 'true'
          output: 'content/posts'
          issue-state: 'open'
          skip-pull-requests: 'true'
      - name: Set output
        continue-on-error: true
        run: echo "ISSUES_ADDED=${{ steps.hugo-with-github-issues.outputs.filenames }}" >> $GITHUB_ENV
      - name: Commit files
        env:
          GITHUB_USERNAME: ${{ github.actor }}
          GITHUB_EMAIL: ${{ github.actor }}@users.noreply.github.com
          ISSUES_ADDED: ${{ env.ISSUES_ADDED }}
        run: |
          date > generated.txt
          git config --local user.email ${{ env.GITHUB_EMAIL }} &&
          git config --local user.name ${{ env.GITHUB_USERNAME }} &&
          git add . &&
          git commit -m "Create new posts from issues, added ${{ env.ISSUES_ADDED }}"
          git push
```

---

