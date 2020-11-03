دیدن لیست مودم ها (سیم کارت)
```
# mmcli -L

Found 1 modems:
	/org/freedesktop/ModemManager1/Modem/0 [Huawei Technologies Co., Ltd.] ME906s-158
```

دیدن مشخصات سیم کارت
```
# mmcli -m 0

/org/freedesktop/ModemManager1/Modem/0 (device id 'c71a363b695b50b428630ece36a34513b11bac19')
  -------------------------
  Hardware |   manufacturer: 'Huawei Technologies Co., Ltd.'
           |          model: 'ME906s-158'
           |       revision: '11.617.04.00.00'
           |      supported: 'gsm-umts'
           |        current: 'gsm-umts'
           |   equipment id: '867160023257771'
  -------------------------
  System   |         device: '/sys/devices/pci0000:00/0000:00:14.0/usb1/1-3'
           |        drivers: 'cdc_ether, option1'
           |         plugin: 'Huawei'
           |   primary port: 'ttyUSB0'
           |          ports: 'ttyUSB0 (at), ttyUSB2 (at), ttyUSB3 (at), wwp0s20f0u3c2 (net)'
  -------------------------
  Numbers  |           own : 'unknown'
  -------------------------
  Status   |           lock: 'sim-pin'
           | unlock retries: 'sim-pin (3), sim-pin2 (3), sim-puk (10), sim-puk2 (10)'
           |          state: 'locked'
           |    power state: 'on'
           |    access tech: 'unknown'
           | signal quality: '0' (cached)
  -------------------------
  Modes    |      supported: 'allowed: 4g; preferred: none
           |                  allowed: 3g; preferred: none
           |                  allowed: 2g; preferred: none
           |                  allowed: 2g, 3g, 4g; preferred: none'
           |        current: 'allowed: 2g, 3g, 4g; preferred: none'
  -------------------------
  Bands    |      supported: 'unknown'
           |        current: 'unknown'
  -------------------------
  IP       |      supported: 'ipv4'
  -------------------------
  SIM      |           path: '/org/freedesktop/ModemManager1/SIM/0'

  -------------------------
  Bearers  |          paths: 'none'
```

زدن pin code برای unlock شدن و اتصال 
```
# mmcli -i 0 --pin=<your_pincode>
# mmcli -m 0 --simple-connect="apn=mcinet"
```

قطع کردن ارتباط
```
# mmcli -m 0 -d                    # completely disable modem
# mmcli -m 0 --simple-disconnect   # only disconnect internet
```

ارسال SMS
```
# mmcli -m 1 --messaging-create-sms="text='Hello world',number='<phone_number>'"
Successfully created new SMS:
	/org/freedesktop/ModemManager1/SMS/5 (unknown)

# mmcli -s 5 --send
```

مشاهده SMS
```
# mmcli -m 0 --messaging-list-sms

Found 6 SMS messages:
	/org/freedesktop/ModemManager1/SMS/0 (received)
	/org/freedesktop/ModemManager1/SMS/1 (received)
	/org/freedesktop/ModemManager1/SMS/2 (received)

# mmcli -m 0 -s /org/freedesktop/ModemManager1/SMS/2
```