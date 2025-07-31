---
title: 'YubiKey (5)다른 머신에서 OpenPGP 키 사용'
description: 'YubiKey로 여러 머신에서 OpenPGP 키 사용'
date: '2025-07-31 17:45:15 +0900'
last_modified_at: '2025-07-31 17:45:15 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

### 1. 조건 : Dependency 설치되어 있어야 함

```
$ sudo apt install gpg scdaemon
```

### 2. YubiKey 카드를 머신에 꽂기

### 3. 카드 읽기 : `gpg --edit-card`

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
URL of public key : https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xd3d7a23522b641fb78acc775000001efcf1a50fa
Login data .......: jmkim
Signature PIN ....: not forced
Key attributes ...: rsa4096 rsa4096 rsa4096
Max. PIN lengths .: 127 127 127
PIN retry counter : 10 0 30
Signature counter : 4
UIF setting ......: Sign=off Decrypt=off Auth=off
Signature key ....: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
Encryption key....: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
Authentication key: FF2F E894 0D74 7810 F045  96FE 3450 E95D 9D03 47A8
      created ....: 2024-06-15 06:10:08
General key info..: [none]

gpg/card>
```

* General key info 부분 `[none]`임을 확인
* `[none]`이 아닐 경우 fetch 필요 없음, `[none]`일 경우 public key fetch가 필요

### 4. Public key 키서버로부터 `fetch`

```
gpg/card> fetch
gpg: requesting key from 'https://keyserver.ubuntu.com/pks/lookup?op=get&search=0xd3d7a23522b641fb78acc775000001efcf1a50fa'
gpg: key 000001EFCF1A50FA: public key "Jongmin Kim" imported
gpg: Total number processed: 1
gpg:               imported: 1

gpg/card> quit
```

### 5. GPG 키 결과 보기 : `gpg --list-secret-keys` 및 `gpg --edit-key 키지문`

```
$ gpg --list-secret-keys
/home/jmkim/.gnupg/pubring.kbx
------------------------------
sec#  rsa4096 2024-06-13 [SCA] [expires: 2029-12-28]
      D3D7A23522B641FB78ACC775000001EFCF1A50FA
uid           [ unknown] Jongmin Kim
uid           [ unknown] Jongmin Kim <jmkim@pukyong.ac.kr>
uid           [ unknown] Jongmin Kim <jmkim@debian.org>
ssb>  rsa4096 2024-06-15 [SEA] [expires: 2027-12-29]
```

`sec`(Master키) 혹은 `ssb`(Sub키) 문구 오른쪽 붙은 마크
* `#` 표시 : 사용 불가(로컬 키링이나 카드 어디에도 없음)
* `>` 표시 : 사용 가능(로컬 키링에 없지만 연결된 카드에 있음)

```
$ gpg --edit-key D3D7A23522B641FB78ACC775000001EFCF1A50FA

Secret subkeys are available.

pub  rsa4096/000001EFCF1A50FA
     created: 2024-06-13  expires: 2029-12-28  usage: SCA
     trust: unknown       validity: unknown
ssb  rsa4096/3450E95D9D0347A8
     created: 2024-06-15  expires: 2027-12-29  usage: SEA
     card-no: xxxx xxxxxxxx
[ unknown] (1). Jongmin Kim
[ unknown] (2)  Jongmin Kim <jmkim@pukyong.ac.kr>
[ unknown] (3)  Jongmin Kim <jmkim@debian.org>
```

카드 번호 `card-no`에 저장되어 있음을 확인 가능
