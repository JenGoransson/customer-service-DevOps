
# ============================================
# CI - Continuous Integration
# Bygger projektet och kör tester automatiskt
# vid push eller pull request till master.
# ============================================

name: CI  # Namnet på vårt GitHub Actions-workflow

# Bestämmer när workflowet ska köras
on:
# Kör när kod skickas till branchen master
push:
branches: [master]

# Kör när en pull request skapas eller uppdateras
# mot branchen master
pull_request:
branches: [master]

# Här definieras de jobb som ska köras
jobs:
build:
# GitHub skapar en virtuell Ubuntu-maskin
# där projektet byggs och testas
runs-on: ubuntu-latest

    steps:
      # 1. Hämtar projektets källkod från GitHub
      # så att workflowet kan arbeta med filerna
      - name: Checkout repository
        uses: actions/checkout@v4

      # 2. Installerar och konfigurerar Java Development Kit 17
      # Temurin är distributionen av Java som används
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'

          # Aktiverar caching av Maven dependencies
          # vilket kan göra framtida byggen snabbare
          cache: 'maven'

      # 3. Bygger projektet med Maven
      # Kompilerar koden, kör tester och skapar
      # exempelvis en .jar- eller .war-fil
      - name: Build with Maven
        run: mvn -B package --file pom.xml

      # ============================================
      # Workflowet är klart om Maven-bygget lyckas.
      # Maven package kör även projektets tester.
      # ============================================