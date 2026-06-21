# 01. EIGRP 기본 구성

## 📌 요구사항
- EIGRP Routing Protocol을 사용하여 모든 네트워크 간 통신
- AS = 100, 자동 요약 기능 사용하지 않음
- EIGRP 라우팅 업데이트가 필요한 Interface로만 EIGRP Packet이 송신되어야 함

---

## 🔧 R1 설정

```cisco
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface serial 1/1
 no passive-interface serial 1/0.123
 no passive-interface fastethernet 0/0
 network 13.13.1.1  0.0.0.0
 network 13.13.9.1  0.0.0.0
 network 13.13.10.1 0.0.0.0
 network 13.13.11.1 0.0.0.0
 network 150.1.13.1 0.0.0.0
!
```

## 🔧 R2 설정

```cisco
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface serial 1/0.123
 network 13.13.2.2  0.0.0.0
 network 13.13.9.2  0.0.0.0
 network 13.13.12.2 0.0.0.0
!
interface serial 1/0.123
 no ip split-horizon eigrp 100
!
```

## 🔧 R3 설정

```cisco
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface serial 1/1
 no passive-interface serial 1/0.123
 no passive-interface fastethernet 0/0
 network 13.13.3.3  0.0.0.0
 network 13.13.9.3  0.0.0.0
 network 13.13.10.3 0.0.0.0
 network 13.13.13.3 0.0.0.0
 network 150.3.13.3 0.0.0.0
!
```

## 🔧 R4 설정

```cisco
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface fastethernet 0/0
 network 13.13.4.4  0.0.0.0
 network 13.13.14.4 0.0.0.0
 network 150.1.13.4 0.0.0.0
!
```

## 🔧 R5 설정

```cisco
router eigrp 100
 no auto-summary
 passive-interface default
 no passive-interface fastethernet 0/0
 network 13.13.5.5  0.0.0.0
 network 13.13.15.5 0.0.0.0
 network 150.3.13.5 0.0.0.0
!
```

---

## ✅ 정보 확인

```cisco
R1# show ip eigrp neighbor   ! 인접성 3개 확인
R2# show ip eigrp neighbor   ! 인접성 2개 확인
R3# show ip eigrp neighbor   ! 인접성 3개 확인
R4# show ip eigrp neighbor   ! 인접성 1개 확인
R5# show ip eigrp neighbor   ! 인접성 1개 확인
```
