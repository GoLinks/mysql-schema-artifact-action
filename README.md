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
