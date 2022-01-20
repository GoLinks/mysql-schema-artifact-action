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
