# MySQL Schema Diff Action

This action is used to check if 2 databases have the same schemas.
This can be used to check if a stage and production database have the same schemas before merging code.

```
inputs:
  hostname1
  username1
  password1
  database1
  hostname2
  username2
  password2
  database2
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
    runs-on: macos-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      
      - name: Install MySQL 5.7
        shell: bash
        run: |
          brew install mysql
          brew install mysql@5.7
          brew unlink mysql && brew link mysql@5.7
          echo 'export PATH="/usr/local/opt/mysql@5.7/bin:$PATH"' >> /Users/runner/.bash_profile
          bash
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
