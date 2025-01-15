# CICDDemo1
This repository has been created to demonstrate how CICD pipeline works

# Step 1: Create a Simple Java Application
Let's start by creating a basic Java application with a unit test.

# Step 2: Push the Code to GitHub
Create a GitHub Repository: Go to GitHub and create a new repository (e.g., java-ci-cd-demo).

Push Code to GitHub: Push your local project to GitHub:
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/java-ci-cd-demo.git
git push -u origin master

# Step 3: Set Up CI/CD Pipeline with GitHub Actions
Create a GitHub Actions Workflow: In your repository, create a directory .github/workflows and then create a file called ci.yml inside it. This YAML file will define the CI/CD pipeline.

Configure the Workflow (ci.yml): Here’s an example of how to set up the CI pipeline for your Java application:

name: Java CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # Step 1: Checkout the code
      - name: Checkout code
        uses: actions/checkout@v2

      # Step 2: Set up Java with the version you need (e.g., OpenJDK 11)
      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'

      # Step 3: Cache dependencies to speed up builds
      - name: Cache Maven dependencies
        uses: actions/cache@v2
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
          restore-keys: |
            ${{ runner.os }}-maven-

      # Step 4: Build the project with Maven
      - name: Build with Maven
        run: mvn clean install

      # Step 5: Run the unit tests
      - name: Run tests with Maven
        run: mvn test
