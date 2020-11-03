### نصب Packer
فایل باینری packer را از [اینجا](https://www.packer.io/downloads.html) دانلود کنید. فایل فشرده را باز کنید و فایل packer را به مسیر usr/local/bin/ منتقل کنید.  
  
### لغت نامه
##### Artifacts
خروجی یک build را می‌گند. نتیجه می‌تونه مجموعه‌ای از ID هاو فایل های باشه که image یک ماشین را تشکیل می دهند.  
  
##### Builds
یک تسک که نتیجه‌اش ساخت یک image برای یک platform باشه.  
  
##### Builders
بخشی از packer که می تواند یک image برای یک platform را بسازد. مانند Virtualbox, VMWare, Amazon EC2 و ...  
  
##### Commands
زیر دستوراتی که به همراه دستور packer یک وظیفه را انجام می‌دهند. مثلا build یا validate.  
```
$ packer validate example.json
Template validated successfully.
```  
  
##### Postprocessors
بخشی از packer که که بر روی نتیجه build یا یک postprocessor دیگه، کاری را انجام می‌دهد. مانند compress که artifact را فشرده می‌کند، یا upload که artifact را upload می‌کند.  
  
##### Provisioners
بخشی از packer که قبل از آنکه ماشین تبدیل به یک image ثابت شود، اقدام به نصب یا پیکربندی نرم افزار بر روی آن می کند. مانند shell scripts, Chef, Puppet, Ansible و ...  
  
##### Templates
یک فایل JSON که یک یا چند build را تعریف می‌کند و می‌تواند شامل بخش‌های مختلف packer که در بالا تعریف شد باشد.  
  
### ساخت یک image
با Packer برای بسترهای زیادی می توان image آماده کرد، Qemu, Amazon, Docker و ... . برای دیدن لیست کامل فرمتهای قابل پشتیبانی به [اینجا](https://www.packer.io/docs/builders/index.html) برید.  
  
### نمونه Template
```
{
    "variables": {
        "aws_access_key": "{{env `AWS_ACCESS_KEY_ID`}}",
        "aws_secret_key": "{{env `AWS_SECRET_ACCESS_KEY`}}",
        "region":         "us-east-1"
    },
    "builders": [
        {
            "access_key": "{{user `aws_access_key`}}",
            "ami_name": "packer-linux-aws-demo-{{timestamp}}",
            "instance_type": "t2.micro",
            "region": "us-east-1",
            "secret_key": "{{user `aws_secret_key`}}",
            "source_ami_filter": {
              "filters": {
              "virtualization-type": "hvm",
              "name": "ubuntu/images/*ubuntu-xenial-16.04-amd64-server-*",
              "root-device-type": "ebs"
              },
              "owners": ["099720109477"],
              "most_recent": true
            },
            "ssh_username": "ubuntu",
            "type": "amazon-ebs"
        }
    ],
    "provisioners": [
        {
            "type": "file",
            "source": "./welcome.txt",
            "destination": "/home/ubuntu/"
        },
        {
            "type": "shell",
            "inline":[
                "ls -al /home/ubuntu",
                "cat /home/ubuntu/welcome.txt"
            ]
        },
        {
            "type": "shell",
            "script": "./example.sh"
        }
    ]
}
```  
  
برای دیدن راهنمای استفاده از هر builder به صفحه [builders](https://www.packer.io/docs/builders/index.html) در مستندات Packer مراجعه کنید.  
برای syntax فایل template ای که تهیه کردید می توانید به شکل زیر عمل کنید
```
$ packer validate example.json
Template validated successfully.
```  
  
برای build گرفتم کافیه به صورت زیر دستور packer را اجرا کنید
```
$ packer build \
    -var 'aws_access_key=YOUR ACCESS KEY' \
    -var 'aws_secret_key=YOUR SECRET KEY' \
    example.json
```  
  
  