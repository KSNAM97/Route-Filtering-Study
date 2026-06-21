# 08. Bogon Filter

## 📖 개념

**Bogon** = 인터넷에 나타나면 안 되는 대역  
사설 / 예약 / 문서용 / 테스트용 대역은 외부로 광고되거나 수신되어서는 안 됨.

---

## 📋 Bogon 대역 요약표

| 대역 | 분류 | 설명 |
|------|------|------|
| `0.0.0.0/8` | This network | "내 네트워크" placeholder |
| `10.0.0.0/8` | 사설 (RFC1918) | 대규모 사내망 |
| `100.64.0.0/10` | CGN (RFC6598) | ISP 가입자 NAT용 |
| `127.0.0.0/8` | Loopback | 자기 자신 |
| `169.254.0.0/16` | Link-local | DHCP 실패 시 자동 할당 |
| `172.16.0.0/12` | 사설 (RFC1918) | 중규모 사내망 |
| `192.0.2.0/24` | TEST-NET-1 | 문서 예제용 |
| `192.168.0.0/16` | 사설 (RFC1918) | 가정/소규모 LAN |
| `198.18.0.0/15` | Benchmark | 장비 성능 테스트용 |
| `198.51.100.0/24` | TEST-NET-2 | 문서 예제용 |
| `203.0.113.0/24` | TEST-NET-3 | 문서 예제용 |
| `224.0.0.0/4` | Multicast | 그룹 통신 |
| `240.0.0.0/4` | Reserved | 미래 예약 (Class E) |

---

## 🔧 Prefix-list 구성

```cisco
ip prefix-list EDITORIAL deny   0.0.0.0/8 le 32           ! this network
ip prefix-list EDITORIAL deny   10.0.0.0/8 le 32          ! RFC1918
ip prefix-list EDITORIAL deny   100.64.0.0/10 le 32       ! CGN (RFC6598)
ip prefix-list EDITORIAL deny   127.0.0.0/8 le 32         ! Loopback
ip prefix-list EDITORIAL deny   169.254.0.0/16 le 32      ! Link-local
ip prefix-list EDITORIAL deny   172.16.0.0/12 le 32       ! RFC1918
ip prefix-list EDITORIAL deny   192.0.2.0/24 le 32        ! TEST-NET-1
ip prefix-list EDITORIAL deny   192.168.0.0/16 le 32      ! RFC1918
ip prefix-list EDITORIAL deny   198.18.0.0/15 le 32       ! Benchmark
ip prefix-list EDITORIAL deny   198.51.100.0/24 le 32     ! TEST-NET-2
ip prefix-list EDITORIAL deny   203.0.113.0/24 le 32      ! TEST-NET-3
ip prefix-list EDITORIAL deny   224.0.0.0/4 le 32         ! Multicast
ip prefix-list EDITORIAL deny   240.0.0.0/4 le 32         ! Reserved
ip prefix-list EDITORIAL permit 0.0.0.0/0 le 32           ! 그 외 허용
```
