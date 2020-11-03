```
kubectl get jobs -o json | jq -r '.items[] | .metadata | [.name,.creationTimestamp | select(. !=  null)] | @csv'
```
  
  
#### لینک‌ها
* [Using jq](http://www.naleid.com/2017/12/17/using-http-apis-on-the-command-line-2-jq.html)