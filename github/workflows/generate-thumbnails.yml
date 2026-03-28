name: Generate PDF Thumbnails

on:
  push:
    paths:
      - 'fichiers/**/*.pdf'
    branches:
      - main

jobs:
  generate-thumbnails:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0
          
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          
      - name: Install system dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev
          
      - name: Install npm dependencies
        run: |
          npm install canvas pdfjs-dist
          
      - name: Generate thumbnails
        run: node scripts/generate-thumbnails.js
        
      - name: Commit thumbnails
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add fichiers/**/*.jpg
          git diff --quiet && git diff --staged --quiet || git commit -m "🤖 Auto-generate PDF thumbnails [skip ci]"
          
      - name: Push changes
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          branch: main
