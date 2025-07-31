---
title: 'YubiKey (3)카드 관리 - gpg 명령어'
description: 'gpg --edit-card로 YubiKey 관리'
date: '2025-07-31 17:38:45 +0900'
last_modified_at: '2025-07-31 17:38:45 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

### 1. Dependency 설치

```
$ sudo apt install gpg scdaemon
```

### 2. `gpg` 명령으로 카드 관리하기 : `gpg --edit-card`

```
$ gpg --edit-card

Reader ...........: 1050:0407:X:0
Application ID ...: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Application type .: OpenPGP
Version ..........: 2.1
Manufacturer .....: Yubico
Serial number ....: xxxxxxxx
Name of cardholder: [not set]
Language prefs ...: [not set]
Salutation .......:
URL of public key : [not set]
Login data .......: [not set]
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 10 0 30
Signature counter : 0
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

gpg/card>
```

### 3. 지원하는 사용자 명령어

```
gpg/card> help
quit           quit this menu
admin          show admin commands
help           show this help
list           list all available data
fetch          fetch the key specified in the card URL
passwd         menu to change or unblock the PIN
verify         verify the PIN and list all data
unblock        unblock the PIN using a Reset Code
openpgp        switch to the OpenPGP app
```

### 4. 지원하는 관리자 명령어

관리자 모드로 진입

```
gpg/card> admin
Admin commands are allowed
```

지원하는 관리자 명령어

```
gpg/card> help
quit           quit this menu
admin          show admin commands
help           show this help
list           list all available data
name           change card holder's name
url            change URL to retrieve key
fetch          fetch the key specified in the card URL
login          change the login name
lang           change the language preferences
salutation     change card holder's salutation
cafpr          change a CA fingerprint
forcesig       toggle the signature force PIN flag
generate       generate new keys
passwd         menu to change or unblock the PIN
verify         verify the PIN and list all data
unblock        unblock the PIN using a Reset Code
factory-reset  destroy all keys and data
kdf-setup      setup KDF for PIN authentication (on/single/off)
key-attr       change the key attribute
uif            change the User Interaction Flag
openpgp        switch to the OpenPGP app
```

### 5. 이름, 로그인, 언어 등 설정

* `name`의 경우 성(surname)-이름(given name) 순서 유의
* `login`의 경우 시스템 라이브러리 연동 시 로그인 스크린에서 키로 시스템 로그인 가능

```
gpg/card> admin
Admin commands are allowed

gpg/card> name
Cardholder's surname: Kim
Cardholder's given name: Jongmin

gpg/card> login
Login data (account name): jmkim

gpg/card> lang
Language preferences: en
```

### 6. 결과 확인

```
$ gpg --edit-card

Reader ...........: 1050:0407:X:0
Application ID ...: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
Application type .: OpenPGP
Version ..........: 2.1
Manufacturer .....: Yubico
Serial number ....: xxxxxxxx
Name of cardholder: Jongmin Kim
Language prefs ...: en
Salutation .......:
URL of public key : [not set]
Login data .......: jmkim
Signature PIN ....: not forced
Key attributes ...: rsa2048 rsa2048 rsa2048
Max. PIN lengths .: 127 127 127
PIN retry counter : 10 0 30
Signature counter : 0
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: [none]
Encryption key....: [none]
Authentication key: [none]
General key info..: [none]

gpg/card> quit
```
