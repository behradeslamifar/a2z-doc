#### Restore mailman backup
```
rsync -avz /usr/local/mailman/lists  root@new-server:/var/lib/mailman/
rsync -avz /usr/local/mailman/data root@new-server:/var/lib/mailman/
rsync -avz /usr/local/mailman/archives root@new-server:/var/lib/mailman/
chmod g+w,o+rx /var/lib/mailman/archives/private/*
chmod o+r /var/lib/mailman/archives/private/ilug/*
chmod -R g+w /var/lib/mailman/archives/
chmod -R g+w /var/lib/mailman/data/
chmod -R g+w /var/lib/mailman/lists/
chgrp  -R list /var/lib/mailman/archives/private/
cd /var/lib/mailman/archives/public
ln -s /var/lib/mailman/archives/private/ilug ilug
find archives/private/ -type d -iname attachments -exec chmod o+x '{}' \;
```

```
/var/lib/mailman/bin/genaliases
```

#### Change list hostname and url
```
/var/lib/mailman/bin/withlist -l -r fix_url mailman -u lists.isfahanlug.org
```

#### Remove list with content
```
rmlist -a listname
```

#### Test mailing list is ok
```
mail -s help mailman-request
```
   

#### Openbase_dir folder
```
/var/lib/mailman/
/usr/lib/cgi-bin/mailman/
```
  
#### =Resources
  * [Howto Install and Configure mailman with Postfix](http://www.howtoforge.com/how-to-install-and-configure-mailman-with-postfix-on-debian-squeeze)
  * [Migrate Server](http://www.debian-administration.org/articles/567)
  * [Migrate Server(2)](https://mail.python.org/pipermail/mailman-users/2007-January/055208.html)
  * [Migrate Server(3)](https://mail.python.org/pipermail/mailman-users/2007-January/055211.html)
  * [Change list name](http://wiki.list.org/pages/viewpage.action?pageId=4030617)