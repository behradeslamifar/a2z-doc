نصب 
```
# apt-get install fprintd libpam-fprintd
```

تعریف انگشت برای کاربر behrad و تست اثر انگشت
```
# fprintd-enroll -f right-index-finger behrad
Using device /net/reactivated/Fprint/Device/0
Enrolling right-index-finger finger.
Enroll result: enroll-stage-passed
Enroll result: enroll-stage-passed
Enroll result: enroll-stage-passed
Enroll result: enroll-stage-passed
Enroll result: enroll-completed

# fprintd-verify behrad
Using device /net/reactivated/Fprint/Device/0
Listing enrolled fingers:
 - #0: right-index-finger
Verify result: verify-match (done)
```

اضافه کردن به pam - به اول فایل اضاف کنید
```
# vi /etc/pam.d/common-auth
auth    [success=3 default=ignore]      pam_fprintd.so try_first_identified debug
...
```