این یک کانفیگ حداقلی است که برای ارسال ایمیل کافیه. معمولا برای ارسال خطاهای اسکریپت ها و این جور کاربردها مناسبه. تنظیمات زیر مربوط به فایل etc/postfix/main.cf/ است.

```
mydomain=linuxmotto.ir
myhostname=server1.$mydomain
myorigin=$myhostname
mynetworks=127.0.0.1
inet_protocols=ipv4
inet_interfaces=127.0.0.1
mydestination=localhost,localdomain.localhost,$myhostname,$mydomain
smtpd_recipient_restrictions = permit_mynetworks,
        permit_sasl_authenticated,
        reject_unauth_destination,
        reject_unauth_pipelining,
        reject_non_fqdn_recipient
append_dot_mydomain=no
compatibility_level = 2
```