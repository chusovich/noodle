#github #mkdocs

The GitHub action publishes the `/docs` folder of the main repo. I believe it will automatically create the `gh-pages` branch. The mkdocs.yml file goes in the root path of the repo.

## GitHub Action

e.g. /.github/workflows/ci.yml

```yaml
name: ci 
on:
  workflow_dispatch:

permissions:
  contents: write
  
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV 
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material 
      - run: mkdocs gh-deploy --force
```

## Configuration File

```yaml
site_name: Personal Wiki
site_url: https://chusovich.github.io/wiki

theme:
  icon:
    logo: material/library
  name: material
  palette: 

    # Palette toggle for light mode
    - scheme: default
      toggle:
        icon: material/brightness-7 
        name: Switch to dark mode

    # Palette toggle for dark mode
    - scheme: slate
      toggle:
        icon: material/brightness-4
        name: Switch to light mode
        
  # Add additional search features
  features:
    - search.highlight
    - search.suggest
    - navigation.instant
    - navigation.tabs
    - content.code.copy

markdown_extensions:
  # Extensions for annotations
  - attr_list
  - md_in_html
  - pymdownx.superfences
  
  # Extensions for code syntax highlighting
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.superfences
  
  # Extension for tables
  - tables
```