---
title: 'YubiKey (0)DebConf25에서 받은 YubiKey 4'
description: '프랑스 DebConf25에서 zigo로부터 YubiKey 4를 받았다.'
date: '2025-07-30 16:07:03 +0900'
image:
  path: /assets/img/posts/2025-07-30-yubikey-by-zigo-in-debconf25/yubikey-4-back.jpg
  alt: YubiKey 4 back
last_modified_at: '2025-07-31 16:07:03 +0900'
layout: post
categories: [YubiKey]
tags: [ko, debconf25, yubikey, linux, debian]
---

프랑스 브레스트의 IMT Atlantique에서 개최된 [DebConf25](https://debconf25.debconf.org)의 이야기다.

[zigo](https://wiki.debian.org/zigo)께서 [Infomaniak](https://www.infomaniak.com) 프린팅이 된 [YubiKey 4](https://support.yubico.com/hc/en-us/articles/360013714599)를 나눠주셨다.

> [Free Infomaniak branded yubikeys for the Debian contributors available at Debcamp/Debconf](https://lists.debian.org/debconf-discuss/2025/06/msg00043.html)

zigo께서는 매 년 Infomaniak 마크가 그려진 YubiKey를 나눠주고 계시다.
나도 대만 신주 DebConf18에서 zigo로부터 YubiKey 4를 받아, 무려 7년 가까이 써오고 있었다.
하지만 내구도가 다했는지 망가졌다.

이번에 한 번 더 나눠 받았다.

![YubiKey 4 front](/assets/img/posts/2025-07-30-yubikey-by-zigo-in-debconf25/yubikey-4-front.jpg)
_YubiKey 4_

이 모델은 최신 모델 [YubiKey 5](https://www.yubico.com/kr/store/yubikey-5-series)에서 소개된 NFC, USB-C 등이 없는, plain한 USB-A 구형 모델이다.

나는 YubiKey에 OpenPGP sub key를 저장하여 사용해왔다. 이번에 새로이 설정하였고, 아래는 그 기록이다.

* [YubiKey (1)공장 초기화](/posts/yubikey-1-factory-reset)
* [YubiKey (2)최초 세팅 - ykman 명령어](/posts/yubikey-2-initial-config-using-ykman)
* [YubiKey (3)카드 관리 - gpg 명령어](/posts/yubikey-3-card-management-using-gpg)
* [YubiKey (4)카드에 OpenPGP 키 넣기](/posts/yubikey-4-move-openpgp-key-using-gpg-keytocard)
* [YubiKey (5)다른 머신에서 OpenPGP 키 사용](/posts/yubikey-5-use-openpgp-key-on-other-machines)

한편, 이번 행사 동안, zigo와 가족분들은 에어비앤비를 빌리셨고, 나는 why, zumbi, ajqlee와 함께 저녁식사에 초대받았다.
zigo의 와이프 분이 중국 분이시어, 아주 맛있는 중화요리를 즐겼다.

zigo는 [Debian OpenStack Team](https://salsa.debian.org/openstack-team)에서 리드하고 계시다.

[Infomaniak](https://www.infomaniak.com)은, zigo가 일하는, 스위스에 위치한 IT 회사로, 클라우드 서비스, 웹 호스팅, 이메일 솔루션, 동영상 스트리밍 플랫폼 등 다양한 사업을 영위하고 있다.
2017년부터 매 년 꾸준히 [높은 금액으로 DebConf에 후원](https://bits.debian.org/2025/01/infomaniak-platinum-debconf25.html)하고 있다.
회사 내부에서는 Proxmox보다 OpenStack을 선호한다는데.
