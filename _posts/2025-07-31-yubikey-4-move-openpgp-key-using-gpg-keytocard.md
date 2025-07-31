---
title: 'YubiKey (4)카드에 OpenPGP 키 넣기'
description: 'gpg keytocard로 카드에 OpenPGP 키 전송'
date: '2025-07-31 17:38:45 +0900'
last_modified_at: '2025-07-31 17:38:45 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

### 1. 전송할 키 선택 : `gpg --edit-key 키지문`

```
$ gpg --edit-key D3D7A23522B641FB78ACC775000001EFCF1A50FA
Secret subkeys are available.

pub  rsa4096/000001EFCF1A50FA
     created: 2024-06-13  expires: 2029-12-28  usage: SCA
     trust: ultimate      validity: ultimate
ssb  rsa4096/3450E95D9D0347A8
     created: 2024-06-15  expires: 2027-12-29  usage: SEA
[ultimate] (1). Jongmin Kim
[ultimate] (2)  Jongmin Kim <jmkim@debian.org>
[ultimate] (3)  Jongmin Kim <jmkim@pukyong.ac.kr>
```

### 2. 전송할 키 선택 : `key 키번호`

* Master 키 선택 : `key 0`
* Sub키 선택 : `key 서브키번호` (첫번째 서브키 : `1`, 현재 선택된 서브키에는 `*` 문자가 옆에 표시됨)

예시 : 첫번째 Sub키 선택

```
gpg> key 1

pub  rsa4096/000001EFCF1A50FA
     created: 2024-06-13  expires: 2029-12-28  usage: SCA
     trust: ultimate      validity: ultimate
ssb* rsa4096/3450E95D9D0347A8
     created: 2024-06-15  expires: 2027-12-29  usage: SEA
[ultimate] (1). Jongmin Kim
[ultimate] (2)  Jongmin Kim <jmkim@debian.org>
[ultimate] (3)  Jongmin Kim <jmkim@pukyong.ac.kr>
```

### 3. 카드로 키 전송 : `keytocard`

GPG 기능 3가지 : Signature(서명), Encryption(암호화), Authentication(인증)

하나의 YubiKey 안에 세 기능 각각 다른 키 설정 가능

* Signature 키 전송 : `1`
* Encryption 키 전송 : `2`
* Authentication 키 전송 : `3`

```
gpg> keytocard
Please select where to store the key:
   (1) Signature key
   (2) Encryption key
   (3) Authentication key
Your selection? 1
```
세 기능 모두 같은 키로 설정하려면, 세 번 반복해서 전송 (1, 2, 3 이런 식)

위의 `keytocard` 시 카드에 키가 바로 전송됨
이후 `save`하면 로컬 키링에서 키 제거됨

```
gpg> save
```

* 만약 로컬 키링에 Master키가 없고 Sub키만 있을 때, 카드에 Sub키를 전송한 경우 `save` 오류가 날 수 있음 (무시 가능, 이 경우 로컬 키링에서 키 제거 안됨)
* 만약 로컬 키링에도 키 유지하려면 `save` 안하고 그냥 `quit`

이후 `gpg` 프롬프트 종료

```
gpg> quit
```

### 4. 카드 결과 확인 : `gpg --edit-card`

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
Key attributes ...: rsa4096 rsa4096 rsa4096
Max. PIN lengths .: 127 127 127
PIN retry counter : 10 0 30
Signature counter : 0
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
Encryption key....: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
Authentication key: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
General key info..:
sub  rsa4096/3450E95D9D0347A8 2024-06-15 Jongmin Kim
sec#  rsa4096/000001EFCF1A50FA  created: 2024-06-13  expires: 2029-12-28
ssb>  rsa4096/3450E95D9D0347A8  created: 2024-06-15  expires: 2027-12-29
                                card-no: xxxx xxxxxxxx
```

### 5. Public key URL 입력 및 fetch

Public key URL 입력 : `url 키주소`

_(참고)우분투 키서버 : `https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x키지문`_

```
$ gpg --edit-card

gpg/card> admin
Admin commands are allowed

gpg/card> url
URL to retrieve public key: https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xd3d7a23522b641fb78acc775000001efcf1a50fa

gpg/card> quit
```

Public key URL 에서 키 받아오기 : `fetch`

```
$ gpg --edit-card

gpg/card> fetch
gpg: requesting key from 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xd3d7a23522b641fb78acc775000001efcf1a50fa'
gpg: key 000001EFCF1A50FA: public key "Jongmin Kim" imported
gpg: Total number processed: 1
gpg:               imported: 1
```
