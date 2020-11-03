### ##### اضافه کردم Policy
به صورت پیشفرض سه policy به نامهای writeonly, readonly و readwrite وجود دارد. در صورتی که نیاز به دسترسی متفاوتی دارید، باید policy مورد نظر خودتون را بسازید.\\
دقت کنید که لیست action ها در مستندات Minio وجود ندارد و باید از مستندات Amazon S3 که در منابع آمده است استفاده کنید.\\

```
$ vi newpolicy.json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Action": [
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::html/*"
      ]
    },
    {
      "Sid": "",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::html"
      ]
    },
    {
      "Sid": "",
      "Action": [
        "s3:ListAllMyBuckets"
      ],
      "Effect": "Allow",
      "Resource": [
        "arn:aws:s3:::*"
      ]
    }
  ]
}
```

Import policy file
```
mc admin --insecure policy add alise_name newpolicy newpolicy.json
```

Create user with new policy

password must lognger than 8 characters
```
mc admin --insecure users add alias_name behrad 12345678 newpolicy
```

Test new user
```
mc config host add alias_name https://127.0.0.1:9000 behrad 12345678 --api s3v4
```



### ##### خطاها
#### شماره ۱
```
$ mc config --insecure host add hussein https://127.0.0.1:9000 hussein 12345678
mc: <ERROR> Unable to initialize new config from the provided credentials. Access Denied.
```

#### شماره ۲

```
$ mc admin --insecure policies add laptop sidewalk sidewalk.json
mc: <ERROR> Cannot add new policy Policy has invalid resource.
```

#### شماره ۳
```
$ mc admin users add laptop behrad 123 sidewalk
mc: <ERROR> Cannot add new user Put https://127.0.0.1:9000/minio/admin/v1/add-user?accessKey=behrad: x509: certificate signed by unknown authority
```




### ##### منابع
در مستندات Minio هیچ بخشی در مورد گزینه هایی که در بخش plicy قابل استفاده است، وجود ندارد. برای این منظور باید از مستندات Amazon استفاده کنید. دقت کنید که همه گزینه ها توسط Minio پشتیبانی نمی شوند.\\
  * [Minio Multi-user Quick Start](https://docs.minio.io/docs/minio-multi-user-quickstart-guide.html)
  * [Amazone S3 Access Policy language](https://docs.aws.amazon.com/AmazonS3/latest/dev/access-policy-language-overview.html)
  * [Amazon S3 Example Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-policies-s3.html)

