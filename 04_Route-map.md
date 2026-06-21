# 04. Route-map

## 📖 개념

- 가장 일반적으로 사용되는 Filtering 기법
- ACL, Prefix-list, Interface를 사용하여 특정 트래픽을 지정한 후 해당 트래픽을 Filtering 하거나 속성/경로를 변경
- Text 기반으로 동작하며, Moduler의 시초

---

## 🔧 동작 순서

1. ACL / Prefix-list / AS-Path-list / Interface 등으로 트래픽 지정
2. Route-map 생성
3. Route-map 안에서 `match` 구문으로 호출
   - **동일 Line** = `OR`
   - **다른 Line** = `AND`
4. `set` 구문으로 Action 수행
5. 지정되지 않은 나머지 트래픽은 **차단**

---

## 📝 예제 (벌금 비유)

| 위반 | 처벌 |
|------|------|
| 쓰레기 무단투기 | 벌금 5만원 |
| 노상 방뇨 | 벌금 5만원 |
| 신호 위반 | 벌금 10만원 |
| 중앙선 침범 | 벌금 10만원 |
| 음주 운전 | 벌금 100만원 |
| 무면허 운전 | 벌금 100만원 |
| 무면허 + 음주 | 구속 |

```cisco
route-map 벌금 permit 10
 match 쓰레기무단투기 노상방뇨
 set 벌금 5만원 부과
!
route-map 벌금 permit 20
 match 신호위반 중앙선침범
 set 벌금 10만원 부과
!
route-map 벌금 permit 30
 match 음주운전 
 match 무면허운전              ! AND
 set 벌금 100만원 부과
!
route-map 벌금 permit 40
 match 음주운전 무면허운전    ! OR
 set 벌금 100만원 부과
!
```
