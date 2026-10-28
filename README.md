name: Generate Neon Snake

on:
  schedule:
    - cron: "0 */12 * * *"   # every 12 hours
  workflow_dispatch:          # lets you run it manually from the Actions tab
  push:
    branches:
      - main

jobs:
  generate:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Generate snake SVG
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake-neon.svg?color_snake=#FF00E5&color_dots=#161B22,#0B3D4A,#00798C,#00C2D4,#00F5FF

      - name: Push to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
