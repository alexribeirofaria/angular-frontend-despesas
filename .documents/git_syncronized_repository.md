✅ sync-angularapp.yml

name: Sync AngularApp folder to another repo

on:
  push:
    branches:
      - develop
    paths:
      - 'AngularApp/**'

jobs:
  sync-folder:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout source repo
      uses: actions/checkout@v3

    - name: Copy AngularApp to target repo
      run: |
        git config --global user.email "you@example.com"
        git config --global user.name "GitHub Action"
        
        mkdir target-repo
        cd target-repo
        git init
        git remote add origin https://<GH_TOKEN>@github.com/SEU_USUARIO/REPO-DESTINO.git
        git fetch origin
        git checkout -b main || git checkout origin/main
        
        rm -rf * .github .gitignore # limpa a pasta destino
        cp -r ../AngularApp/* .     # copia o conteúdo da AngularApp
        
        git add .
        git commit -m "Sync AngularApp from source repo" || echo "No changes to commit"
        git push origin main

    env:
      GH_TOKEN: ${{ secrets.TOKEN_DESTINO }}
