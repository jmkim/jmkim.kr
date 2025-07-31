---
title: 'YubiKey (2)최초 세팅 - ykman 명령어'
description: 'ykman으로 YubiKey 비밀번호 설정'
date: '2025-07-31 17:35:29 +0900'
last_modified_at: '2025-07-31 17:35:29 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

### 1. 관리자 비밀번호 설정

관리자 역할 : 비밀번호 변경, 키 관리

```
$ ykman openpgp access change-admin-pin
Enter Admin PIN:
New Admin PIN:
Repeat for confirmation:
Admin PIN has been changed.
```

### 2. 사용자 비밀번호 설정

사용자 역할 : 키 사용 (키 사용할 때 마다 비밀번호 프롬프트가 뜸)

```
$ ykman openpgp access change-pin
Enter PIN:
New PIN:
Repeat for confirmation:
User PIN has been changed.
```

### 3. 리셋 코드 설정

리셋코드 : 사용자 비밀번호 변경용 코드

* 사용자와 관리자가 다른 사람일 때 용이한 기능
* 사용자가 관리자 없이도 사용자 비밀번호 변경 가능
* 관리자가 사용자에게 YubiKey에 키를 넣어 배포할 때 리셋코드를 종이에 적어 함께 배포
* 리셋코드 종이 유출 시 누구나 사용자 비밀번호 변경하여 키 사용 가능하므로 주의

```
$ ykman openpgp access change-reset-code
Enter Admin PIN:
New Reset Code:
```

사용자 == 관리자면 보통 필요없어서 세팅 안함

_(참고) 리셋코드로 사용자 비밀번호 변경하는 방법_

```
$ ykman openpgp access unblock-pin
Enter Reset Code:
New PIN:
Repeat for confirmation:
User PIN has been changed.
```

### 4. 비밀번호 최대 실패 횟수 설정

```
$ ykman openpgp access set-retries -h
Usage: ykman openpgp access set-retries [OPTIONS] PIN-RETRIES RESET-CODE-RETRIES ADMIN-PIN-RETRIES

  Set the number of retry attempts for the User PIN, Reset Code, and Admin PIN.
```

명령어 : `ykman openpgp access set-retries 사용자비밀번호 리셋코드 관리자비밀번호`

예시 : `ykman openpgp access set-retries 10 20 30`

* 사용자 비밀번호 10회 이상 초과 시 카드 잠금
* 리셋코드 20회 이상 초과 시 카드 잠금
* 관리자 비밀번호 30회 이상 초과 시 카드 잠금

```
$ ykman openpgp access set-retries 10 20 30
Enter Admin PIN: 
Set PIN retry counters to: 10 20 30? [y/N]: y
Number of PIN/Reset Code/Admin PIN retries set.
```

리셋코드 + 관리자 비밀번호 둘 다 초과하여 카드가 잠기면 초기화 필요 ([YubiKey (1)공장 초기화](/posts/yubikey-1-factory-reset) 참조)
