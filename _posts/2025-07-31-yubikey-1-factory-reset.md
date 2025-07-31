---
title: 'YubiKey (1)공장 초기화'
description: 'YubiKey 공장 초기화 하는 방법'
date: '2025-07-31 16:54:15 +0900'
last_modified_at: '2025-07-31 16:54:15 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

참고하실 자료

* [YubiKey 4 소개](https://support.yubico.com/hc/en-us/articles/360013714599)
* [YubiKey 4 리셋 방법](https://support.yubico.com/hc/en-us/articles/360013707640)
* [YubiKey 5 리셋 방법](https://support.yubico.com/hc/en-us/articles/360013757959)

YubiKey 4 안에는 5개 앱이 있다.

* OTP
* FIDO U2F
* PIV (Smart Card)
* OATH
* OpenPGP

첫 사용 이전에 앱을 각각 Factory Default로 리셋(공장 초기화) 하고 쓰는 게 좋다.

## YubiKey 매니저 설치하기

```
$ sudo apt install yubikey-manager
```

YubiKey 매니저 명령어 : `ykman`

```
$ ykman -h
Usage: ykman [OPTIONS] COMMAND [ARGS]...

Commands:
  info     show general information
  list     list connected YubiKeys
  script   run a python script
  config   configure the YubiKey, enable or disable applications
  fido     manage the FIDO applications
  hsmauth  manage the YubiHSM Auth application
  oath     manage the OATH application
  openpgp  manage the OpenPGP application
  otp      manage the YubiOTP application
  piv      manage the PIV application
```

### 1. OTP 공장초기화

1. 카드 현황 확인

```
$ ykman otp info
Slot 1: programmed
Slot 2: empty
```

2. `programmed` 된 슬롯 제거

```
$ ykman otp delete 1
Do you really want to delete the configuration of slot 1? [y/N]: y
Configuration slot 1 deleted.

$ ykman otp delete 2
ERROR: Not possible to delete an empty slot.
```

3. 결과 확인

```
$ ykman otp info
Slot 1: empty
Slot 2: empty
```

### 2. FIDO U2F 공장초기화

> This application is not configurable and cannot be reset.
_[Resetting the YubiKey 4 or YubiKey NEO to factory defaults](https://support.yubico.com/hc/en-us/articles/360013707640)_

```
$ ykman fido info
PIN: Not supported

$ ykman fido reset
ERROR: This YubiKey does not support FIDO reset.
```

YubiKey 4는 초기화를 지원하지 않는다. 애초에 카드 안에 PIN을 저장하는 구조가 아니라서 초기화가 필요하지 않다.

### 3. PIV (Smart Card) 공장초기화

1. 카드 현황 확인

```
$ ykman piv info
PIV version:              4.3.5
PIN tries remaining:      3
Management key algorithm: TDES
CHUID: No data available
CCC:   No data available
```

2. 초기화

```
$ ykman piv reset
WARNING! This will delete all stored PIV data and restore factory settings. Proceed? [y/N]: y
Resetting PIV data...
Reset complete. All PIV data has been cleared from the YubiKey.
Your YubiKey now has the default PIN, PUK and Management Key:
    PIN:    123456
    PUK:    12345678
    Management Key:    010203040506070801020304050607080102030405060708
```

3. 결과 확인

```
$ ykman piv info 
PIV version:              4.3.5
PIN tries remaining:      3
Management key algorithm: TDES
CHUID: No data available
CCC:   No data available
```

초기 상태였다면, 1과 결과화면은 같을 것이다.

### 4. OATH 공장초기화

1. 카드 현황 확인

```
$ ykman oath info
OATH version:        4.3.5
Password protection: disabled
```

2. 초기화

```
$ ykman oath reset 
WARNING! This will delete all stored OATH accounts and restore factory settings. Proceed? [y/N]: y
Resetting OATH data...
Reset complete. All OATH accounts have been deleted from the YubiKey.
```

3. 결과 확인

```
$ ykman oath info 
OATH version:        4.3.5
Password protection: disabled
```

초기 상태였다면, 1과 결과화면은 같을 것이다.

### 5. OpenPGP 공장초기화

1. 카드 현황 확인

```
$ ykman openpgp info
OpenPGP version:            2.1
Application version:        4.3.5
PIN tries remaining:        3
Reset code tries remaining: 0
Admin PIN tries remaining:  3
Require PIN for signature:  Once
KDF enabled:                False
```

2. 초기화

```
$ ykman openpgp reset
WARNING! This will delete all stored OpenPGP keys and data and restore factory settings. Proceed? [y/N]: y
Resetting OpenPGP data, don't remove the YubiKey...
Reset complete. OpenPGP data has been cleared and default PINs are set.
PIN:         123456
Reset code:  NOT SET
Admin PIN:   12345678
```

3. 결과 확인

```
$ ykman openpgp info
OpenPGP version:            2.1
Application version:        4.3.5
PIN tries remaining:        3
Reset code tries remaining: 0
Admin PIN tries remaining:  3
Require PIN for signature:  Once
KDF enabled:                False
```

초기 상태였다면, 1과 결과화면은 같을 것이다.
