### Terminology
**fast-forward**: به merge ای گفته می شود که تنها HEAD به کامیت آخر منتقل می شود. در حقیقت commit انجام شده به آخرین commit اضافه می شود.  
  
### branch
```
$ git branch --all                                   # List branchs

$ git branch -d branch_name                          # Delete local branch
$ git branch -D branch_name                          # same as -d with --force

$ git push --delete <remote_name> <branch_name>      # Delete remote branch
$ git push <remote_name> --delete <branch_name>
$ git push <remote_name> :<branch_name>
```  
  
### git rebase
در صورتی که بخواهیم چند تا commit را از مستر حذف کنیم و در یک branch قرار دهیم.
```
git branch rbd-provisioner <commit_id>
git rebase --onto <start commit_id> <end_commit_id>
... resolve confilict (use git status and git add) ...
git rebase  --continue                               # continue after resolve confilicts
git push -u origin rbd-provisioner                   # push branch
git push --force-with-lease                          # push master
```  
  
#### =git tag
```
git tag <tagname>
git push --tags
```  
  
  