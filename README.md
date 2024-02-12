# MySQL Schema Artifact Action

This action is used to create a SQL Dump file with no exteraneous data except the raw table structures
This can be used to check if a stage and production database have the same schemas before merging code.

```
inputs:
  hostname: The host name to connect to the database
  username: The username to connect to the database
  password: The password to connect to the database
  database: The name of the database to dump
  file: The name of the file that will be dumped, and used as the artifact.
```

## Example

```yaml
# This is a basic workflow to help you get started with Actions

name: Check Databases

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v4

      - name: Install MySQL 5.7
        shell: bash
        run: |
          sudo apt-get update
          sudo apt-get install -y mysql-server
          sudo systemctl start mysql.service
          mysql --version
          mysqldump --version
      # Runs a single command using the runners shell
      - name: Check MySQL schemas
        uses: ./
        with:
          file: db.sql
          hostname: ${{ secrets.HOSTNAME }}
          username: ${{ secrets.USERNAME }}
          password: ${{ secrets.PASSWORD }}
          database: ${{ secrets.DATABASE }}
      - uses: actions/upload-artifact@v2
        with:
          name: db.sql
          path: ./db.sql
```
