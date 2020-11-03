آزمایش یک Resource Agent جدید
```
# ocf-tester -n /usr/lib/ocf/resource.d/heartbeat/<resource_agent>
```

```
# export OCF_ROOT=/usr/lib/ocf; bash -x /usr/lib/ocf/resource.d/heartbeat/<agent_name> start
```

عیب یابی 
```
# pcs resource show <agent_name>
```

```
pcs resource debug-start <agent_name>
pcs resource debug-stop <agent_name>
pcs resource debug-monitor <agent_name>
```