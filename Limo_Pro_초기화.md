# Limo 손상시 원상 복구 가이드
## 0. 장비 버전 확인표

아래 시리얼 번호를 기준으로 Limo의 저장장치 버전을 구분합니다.

| 시리얼 번호   | 저장장치 버전    | 비고        |
| -------- | ---------- | --------- |
| LM003695 | SSD 버전     | 3000번대 이상 |
| LM002502 | SD Card 버전 | 2000번대    |
| LM003698 | SSD 버전     | 3000번대 이상 |
| LM002507 | SD Card 버전 | 2000번대    |
| LM003697 | SSD 버전     | 3000번대 이상 |
| LM003700 | SSD 버전     | 3000번대 이상 |

> **구분 기준**
> 시리얼 번호가 **LM003000번대 이상이면 SSD 버전**,
> 시리얼 번호가 **LM002000번대이면 SD Card 버전**으로 분류합니다.

---

## 0.1 바로가기

사용 중인 Limo의 저장장치 버전에 따라 아래 항목으로 이동하여 작업을 진행하십시오.

* [1. SD Card 버전](#1-sd-card-버전)
* [2. Limo Pro Image 버전](#2-limo-pro-image-버전)
* [3. SSD 버전](#3-ssd-버전)
* [4. 주의사항](#4-주의사항)
* [5. 버전 선택 기준](#5-버전-선택-기준)
* [6. 전체 작업 요약](#6-전체-작업-요약)

---

## 0.2 상세 사진 참고 링크

상세 분해 사진과 실제 장비 분해 과정 이미지는 아래 링크를 참고하십시오.

* Limo 이미지 버전 및 업데이트 Notion 문서
  https://goofy-pleasure-a84.notion.site/Limo-460a0c2f2b684b4ab9231c9534359e0e

---

현재 사용 중인 **Limo Pro**는 저장장치 이미지를 다시 설치하거나 복제하는 방식으로 시스템을 초기화할 수 있습니다.

Limo Pro는 저장장치 방식에 따라 크게 다음 두 가지 버전으로 구분됩니다.

1. **SD Card 버전**
2. **SSD 버전**

---

# 1. SD Card 버전

## 1.1 개요

기존 Limo Pro는 Jetson Orin Nano에 SD 카드를 사용하여 시스템을 구성합니다.

다만 현재 Jetson Orin Nano 환경에서 SD 카드 이미지가 손상되는 현상이 확인되어, **2025년 이후 Limo를 구매한 고객부터는 SSD 기반 세팅을 제공**하고 있습니다.

따라서 현재 SD 카드 버전의 Limo를 사용 중인 경우, 가능하면 **SSD 버전으로 교체하는 것을 권장**합니다.

그래도 SD 카드 버전을 계속 사용해야 하는 경우에는 아래 절차에 따라 Limo 시스템을 초기화하거나 업데이트할 수 있습니다.

> **참고:**
> SD 카드 분리 과정의 상세 분해 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

## 1.2 Limo Pro SD 카드 버전 업데이트 방법

현재 보유 중인 Limo Pro의 시스템을 업데이트하려면, Limo 내부의 SD 카드를 분리한 뒤 제공된 Limo Pro 이미지 파일을 SD 카드에 다시 기록해야 합니다.

전체 절차는 다음과 같습니다.

---

## 1.3 준비물

* Limo Pro 이미지 파일 `.gz`
* SD 카드 리더기
* Windows PC
* SD Card Formatter
* Rufus
* Limo 본체
* 기본 공구 세트

---

## 1.4 Limo Pro 이미지 다운로드

아래의 **Limo Pro Image 버전 목록**에서 필요한 버전의 이미지 설치 링크를 통해 `.gz` 형식의 이미지 파일을 다운로드합니다.

---

## 1.5 Limo에서 SD 카드 분리

Limo 본체에서 SD 카드를 분리합니다.

SD 카드 분리 순서는 다음과 같습니다.

1. LiDAR 캐노피 제거
2. Limo 상판 제거
3. Jetson Orin Nano 분리
4. SD 카드 분리

> **상세 사진 참고:**
> LiDAR 캐노피 제거, 상판 제거, Jetson Orin Nano 분리, SD 카드 분리 과정의 상세 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

## 1.6 SD 카드 이미지 덮어쓰기

### 1.6.1 SD 카드 PC 연결

SD 카드 리더기를 사용하여 분리한 SD 카드를 Windows PC에 연결합니다.

```text
SD 카드 → SD 카드 리더기 → Windows PC
```

---

### 1.6.2 SD 카드 포맷

SD 카드 포맷은 **SD Card Formatter**를 사용합니다.

다운로드 링크:

https://www.sdcard.org/downloads/formatter/sd-memory-card-formatter-for-windows-download/

작업 순서는 다음과 같습니다.

1. SD Card Formatter를 설치합니다.
2. SD 카드와 PC를 연결합니다.
3. `Select card` 항목에서 Ubuntu가 설치된 SD 카드를 선택합니다.
4. `Format` 버튼을 클릭합니다.
5. 포맷이 완료될 때까지 기다립니다.

> **주의:** 다른 저장장치를 선택하면 데이터가 삭제될 수 있으므로 반드시 SD 카드 장치를 정확히 확인한 뒤 진행해야 합니다.

---

### 1.6.3 Rufus로 이미지 쓰기

SD 카드 포맷이 완료되면 Rufus를 사용하여 Limo Pro 이미지를 SD 카드에 덮어씁니다.

Rufus 다운로드 링크:

https://rufus.ie/ko/

작업 순서는 다음과 같습니다.

1. Rufus를 다운로드합니다.
2. Rufus를 실행합니다.
3. `장치` 항목에서 SD 카드를 선택합니다.
4. `선택` 버튼을 클릭하여 다운로드한 Limo Pro 이미지 파일을 선택합니다.
5. `시작` 버튼을 클릭합니다.
6. 이미지 쓰기가 완료될 때까지 기다립니다.

이미지 쓰기 작업은 환경에 따라 약 **1시간 30분** 정도 소요될 수 있습니다.

> **주의:** 이미지 쓰기 중에는 SD 카드 리더기를 분리하지 마십시오.

---

## 1.7 SD 카드 재장착 및 부팅 확인

이미지 쓰기가 완료되면 SD 카드를 Jetson Orin Nano에 다시 장착합니다.

이후 분해한 순서의 반대로 Limo를 조립한 뒤 전원을 켜고 정상 부팅 여부를 확인합니다.

---

# 2. Limo Pro Image 버전

## 2.1 Limo Pro Version 1.0.0

### 이미지 설치 링크

https://drive.google.com/file/d/1urNVOPIRPr2KSU8PWyJ2K3sZP5F5mfZT/view?usp=sharing

### 업데이트 일자

* 2024년 2월 16일

### 업데이트 내용

* Jetpack 5.1.2 Full Package 설치
* SD 카드 용량 128GB
* Isaac ROS 설치
* Camera driver 설치
* Limo driver 설치
* LiDAR driver 설치
* URDF 및 TF 구성
* EKF를 통한 로봇 위치 추정 시스템 구성
* Track 주행용 Line Tracking Application 추가

  * Detect line
  * E-Stop
  * Detect hump
  * Limo control
* Limo 간단 제어 Application 추가

  * Move limo
* Limo 사진 촬영 Application 추가

  * Take a picture

---

## 2.2 Limo Pro Version 1.0.1

### 이미지 설치 링크

https://drive.google.com/file/d/1eC4UKK1IN1cDSRFUdEkOjpXANjv10vq_/view?usp=drive_link

### 업데이트 일자

* 2024년 3월 14일

### 업데이트 내용

* Limo TF 및 URDF 오류 수정
* Camera Mount 추가
* Move to Pose Application 추가
* 통합 드라이버 launch 파일 기능 추가
* ROS2 Cartographer SLAM 기능 추가
* ROS1 설치 및 workspace 추가
* Line Tracking Application 아이콘 추가

---

## 2.3 Limo Pro Version 1.0.2

### 이미지 설치 링크

https://drive.google.com/file/d/1UnPZjtar07zSyl8vktVb_g0HUBGM0Lt3/view?usp=drive_link

### 업데이트 일자

* 2024년 4월 15일

### 업데이트 내용

* Camera Driver 개선

  * Image Topic과 PointCloud Data 분리
* Differential 방식 Navigation 시스템 세팅
* Navigation Application 추가

  * Patrol Limo
  * Move Through Poses

---

## 2.4 Limo Pro Version 1.0.3

### 이미지 설치 링크

보안상의 이유로 삭제되었습니다.
해당 버전은 추후 **Version 1.0.4**로 제공될 예정입니다.

### 업데이트 일자

* 2024년 5월 20일

### 업데이트 내용

* Track 주행 Application 중 Debugging Image에 Reference Lane 추가
* Track 주행 Application Parameter 수정
* Take Picture Application 코드 수정
* 사진 저장 위치 수정
* YOLOv8 기반 객체 인식 기능 포팅
* Deep Learning 기반 Lane Detection 기능 추가
* 차단기 제어 기능 추가
* Limo Ackermann 모드 Driver 개발

---

## 2.5 Limo Pro Version 1.0.4

### 이미지 설치 링크

https://drive.google.com/file/d/1Cxr9Vqbs-NV_q1xkxO3JCwIn-sxv9pJb/view?usp=drive_link

### 업데이트 일자

* 2024년 7월 24일

### 업데이트 내용

* Camera Driver 변경
* `limo_ros2_application` 패키지 개선
* Track 주행 Application에서 IMU 관련 부분 삭제
* Limo Driver Package 개선

  * Ackermann launch 파일 분리
  * Mecanum launch 파일 분리
  * IMU data 수신 부분 개선
* 차선 인식 아이콘 실행 방식 변경

---

# 3. SSD 버전

## 3.1 개요

2025년 이후 출고되는 Limo Pro는 SSD를 이용하여 시스템 세팅을 진행하고 있습니다.

Limo 초기화가 필요한 고객께서는 Wego 측에 문의한 뒤, 제공받은 원본 SSD를 이용하여 이미지를 복제해 사용할 수 있습니다.

SSD 버전의 이미지 복제 방법은 다음과 같습니다.

> **참고:**
> SSD 교체 과정의 상세 분해 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

## 3.2 SSD 이미지 세팅 준비물

* 공구 세트
* SSD 2개

  * 원본 이미지 SSD: Wego에서 제공
  * 복제 대상 SSD: 128GB 이상 권장
* SSD 복제기
* Limo Screw Driver

---

## 3.3 Limo Pro SSD 업데이트 방법

### 3.3.1 SSD 복제

SSD 복제기를 사용하여 Wego에서 제공받은 원본 SSD의 이미지를 사용자의 SSD로 복제합니다.

작업 순서는 다음과 같습니다.

1. 원본 SSD를 SSD 복제기의 Source 슬롯에 장착합니다.
2. 복제 대상 SSD를 Target 슬롯에 장착합니다.
3. 복제기의 복제 기능을 실행합니다.
4. 복제가 완료될 때까지 기다립니다.
5. 복제 완료 후 SSD를 분리합니다.

> **주의:** Source와 Target 위치를 반대로 장착하면 원본 SSD 데이터가 손상될 수 있으므로 반드시 장착 방향을 확인해야 합니다.

---

### 3.3.2 Limo 상판 분리

복제한 SSD를 Limo에 장착하기 위해 Limo 상판을 분리합니다.

작업 순서는 다음과 같습니다.

1. Limo 전원을 끕니다.
2. 전방 캐노피를 분리합니다.
3. Bluetooth/Wi-Fi 안테나를 분리합니다.
4. 상판 고정 너트를 풉니다.
5. Limo 상판을 제거합니다.

> **상세 사진 참고:**
> 전방 캐노피 분리, Bluetooth/Wi-Fi 안테나 분리, 상판 제거 과정의 상세 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

### 3.3.3 제어기 케이스 제거

상판을 제거한 후 SSD가 장착된 제어기 케이스를 분리합니다.

작업 순서는 다음과 같습니다.

1. 제어기 케이스의 고정 나사를 확인합니다.
2. 고정 나사를 풉니다.
3. 케이스를 조심스럽게 분리합니다.
4. 내부 케이블이 손상되지 않도록 주의합니다.

> **상세 사진 참고:**
> 제어기 케이스 제거 과정의 상세 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

### 3.3.4 SSD 교체

기존 SSD를 분리하고 복제한 SSD로 교체합니다.

작업 순서는 다음과 같습니다.

1. 기존 SSD의 장착 위치를 확인합니다.
2. 기존 SSD를 분리합니다.
3. 복제한 SSD를 같은 위치에 장착합니다.
4. 연결 상태를 확인합니다.
5. 제어기 케이스를 다시 조립합니다.
6. Limo 상판을 다시 조립합니다.
7. Bluetooth/Wi-Fi 안테나를 다시 장착합니다.
8. 전원을 켜고 정상 부팅 여부를 확인합니다.

> **상세 사진 참고:**
> 기존 SSD 분리 및 복제한 SSD 장착 과정의 상세 사진은 문서 상단의 Notion 참고 링크를 확인하십시오.

---

# 4. 주의사항

## 4.1 저장장치 선택 주의

SD 카드 포맷, Rufus 이미지 쓰기, SSD 복제 작업에서는 저장장치 선택이 매우 중요합니다.

잘못된 저장장치를 선택하면 기존 데이터가 삭제될 수 있습니다.

---

## 4.2 작업 중 전원 차단 금지

이미지 쓰기 또는 SSD 복제 작업 중 전원이 차단되면 저장장치 이미지가 손상될 수 있습니다.

작업 중에는 다음 사항을 확인해야 합니다.

* PC 절전 모드 해제
* SD 카드 리더기 분리 금지
* SSD 복제기 전원 차단 금지
* 복제 완료 표시 확인 후 저장장치 분리

---

## 4.3 SD 카드 버전 사용 시 권장사항

SD 카드 버전은 장기간 운용 시 이미지 손상 가능성이 있으므로, 안정적인 운용을 위해 SSD 버전으로 전환하는 것을 권장합니다.

---

# 5. 버전 선택 기준

| 구분               | 권장 버전                  | 비고                 |
| ---------------- | ---------------------- | ------------------ |
| LM003000번대 이상 장비 | SSD 버전                 | SSD 복제 방식 사용       |
| LM002000번대 장비    | SD Card 버전             | SD 카드 이미지 쓰기 방식 사용 |
| SD 카드 버전 사용 중    | Limo Pro Version 1.0.4 | 최신 SD 카드 이미지       |
| 2025년 이후 출고 장비   | SSD 버전                 | Wego 제공 SSD 기준     |
| 기존 이미지 손상 발생     | SSD 전환 권장              | 장기 운용 안정성 확보       |
| 교육 및 실습용 복구      | SD 카드 이미지 재설치 가능       | Rufus 사용           |

---

# 6. 전체 작업 요약

## 6.1 SD Card 버전

1. Limo Pro 이미지를 다운로드합니다.
2. Limo 전원을 종료합니다.
3. LiDAR 캐노피를 제거합니다.
4. Limo 상판을 제거합니다.
5. Jetson Orin Nano를 분리합니다.
6. SD 카드를 분리합니다.
7. SD 카드를 Windows PC에 연결합니다.
8. SD Card Formatter로 SD 카드를 포맷합니다.
9. Rufus로 Limo Pro 이미지를 기록합니다.
10. SD 카드를 재장착합니다.
11. Limo를 재조립합니다.
12. 부팅을 확인합니다.

---

## 6.2 SSD 버전

1. Wego 제공 원본 SSD를 준비합니다.
2. 복제 대상 SSD를 준비합니다.
3. SSD 복제기로 이미지를 복제합니다.
4. Limo 전원을 종료합니다.
5. 전방 캐노피 및 안테나를 분리합니다.
6. Limo 상판을 제거합니다.
7. 제어기 케이스를 제거합니다.
8. 기존 SSD를 분리합니다.
9. 복제한 SSD를 장착합니다.
10. Limo를 재조립합니다.
11. 부팅을 확인합니다.
