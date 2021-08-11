---
layout: post
title: "[TIL]TCP 3-way Handshake와 네트워크 레이턴시(latency)"
date: "2021-08-11"
categories:
  - TIL
excerpt: |
  TCP는 신속보다는 정확한 데이터 교환에 초점이 맞춰진 프로토콜로, 3-way Handshake를 통해 클라이언트/서버 간 통신이 진행된다. 따라서 최초 1회 왕복 패킷 교환이 반드시 수반되어야 하며, 이는 웹 브라우징 레이턴시의 주된 원인이 된다. 따라서, 보다 최적화된 애플리케이션을 구성하기 위해서는 매번 새 TCP connection을 맺는 것보다 기존 connection을 재사용할 필요가 있다.
feature_text: |
  ## TCP의 3-way Handshake로 인한 레이턴시 증가를 최소화하기 위한 기존 connection 재사용 필요성
  TCP는 신뢰의 프로토콜이지만, 그만큼 비용도 수반된다. 3-way Handshake는 레이턴시의 주된 원인이며, 신뢰성을 확보하기 위해 수반되는 일종의 비용이다. 이를 절감하기 위해서는 매번 새 TCP connection을 생성하기 보다는 기존 connection을 재사용해야 한다.
feature_image: "https://images.unsplash.com/photo-1545987796-200677ee1011?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80"
image: "https://images.unsplash.com/photo-1545987796-200677ee1011?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80"
---

우리가 흔히 사용하는 인터넷 환경은 TCP/IP 프로토콜을 기반으로 한다. 여게서 IP는 서로 다른 호스트 간 라우팅 및 주소 할당에 관련된 프로토콜이며, TCP는 데이터 통신의 신뢰성을 보장하는 프로토콜이다.

TCP는 속도보다는 신뢰성을 중요시하는 프로토콜이다. 빠르게 전달되는 부정확한 데이터보다는 한 치의 오차도 없는 정확한 데이터의 확실한 전송을 목표로 설계된 프로토콜이다. 따라서 TCP는 이런 완전무결한 데이터의 송신을 위해 3-way handshake와 같은 고도의 신뢰성 확보 매커니즘을 도입하고 있다.

모든 TCP 연결은 3-way handshake를 통해 맺어진다. 클라이언트와 서버는 아래 그림과 같이 최초 1회 패킷 왕복이 있은 후, 비로소 데이터를 주고 받을 수 있게 된다. 즉 클라이언트에서 서버로 1회, 서버에서 다시 클라이언트로 1회 총 2회의 레이턴시를 겪게 되는 것이다. 3-way handshake로 인해 낭비되는 왕복 레이턴시는 결코 작지 않다.

<img src='https://github.com/ChaeWonKong/image-resource/blob/master/3-way-handshake.png?raw=true' width='300px' />

만약 뉴욕에서 런던으로 네트워크 연결을 한다고 하면, 진공 상태에서는 신호 전달에 19ms가, 광섬유를 통한 신호전달에는 28ms가 소요된다. 이상적인 상태인 진공 상태에서 빛의 속도는 약 초속 30만 km이지만, 패킷은 진공상태가 아닌 광섬유 케이블이나 구리 케이블을 통해 전파된다. 이 중 가장 빠르게 신호(빛)를 전달할 수 있는 광섬유 케이블을 이용하더라도 빛의 속도보다는 느린 속도로만 패킷을 전달할 수 있다.

뉴욕의 클라이언트가 런던의 서버와 통신하기 위해서는 SYN 패킷을 전달하는데 28ms, 런던의 서버로부터 시퀸스번호를 1만큼 증가시킨 값과 또 다른 무작위 시퀸스 번호를 되돌려 받는데 28ms가 소요되어, 왕복 56ms가 낭비되게 된다. 이렇게 SYN ACK 패킷을 주고받은 후에야 비로소 목표로 하던 통신을 수행할 수 있게 된다.

이런 이유에서 TCP Handshake는 웹 브라우징 레이턴시의 주된 이유로 지목된다. 따라서 지연시간을 최소화 하기 위해서는 매번 새로운 TCP connection을 맺는 것보다, 기존 connection을 재사용하는 것이 매우 중요하다.

### 출처

- 네트워킹과 웹 성능 최적화 기법, 일리아 그레고릭, 인사이트
