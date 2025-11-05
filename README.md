# PX4_MAVLink-Benign-data-set-analysis

# MAVLink Data Analysis & GPS Spoofing Experiment
**PX4 기반 드론의 MAVLink 데이터 분석 및 GPS Spoofing 실험**

## Introduction
이 프로젝트는 **PX4 드론의 MAVLink 데이터를 분석**하고,  
**HackRF를 활용한 GPS 스푸핑 및 재밍 실험**을 수행하여 드론의 비행에 미치는 영향을 연구하는 것을 목표로 한다.

## 📡 MAVLink Data Logging & Analysis
### MAVLink란?
MAVLink(Micro Air Vehicle Link)는 **드론과 지상통제시스템(GCS) 간의 통신을 위한 경량 메시지 프로토콜**이다.  
비행 중 발생하는 **GPS 데이터, 속도, 고도, 자세(Attitude), 조종기 입력 값** 등을 전송하는 역할을 한다.
0
### MAVLink 데이터 저장 과정
MAVLink 데이터는 기본적으로 자동 저장되지 않으며,  
로그 데이터는 **PX4 내부 SD 카드** 또는 **GCS(QGroundControl)에서 수집한 TLOG 파일**을 통해 기록된다.

- **TLOG 파일 (`.tlog`)**  
  - MAVLink 원본 데이터를 기록  
  - QGroundControl(QGC)에서 실시간으로 저장됨  
  - 비행 중 발생한 모든 MAVLink 메시지를 포함  
  - **저장 위치**: `C:\Users\사용자이름\Documents\QGroundControl\Telemetry\`

- **Vehicle CSV (`vehicle.csv`)**  
  - TLOG 데이터를 CSV 파일로 변환한 것  
  - MAVLink 메시지 중 **위치, 속도, 고도 등 핵심 데이터만 포함**  
  - 엑셀, Python(Pandas), MATLAB 등에서 분석 가능

## 🛰️ GPS Spoofing & Jamming Experiment
### 실험 개요
**HackRF와 GPS-SDR-SIM을 이용하여 PX4 드론의 GPS 신호를 변조(스푸핑)하거나 방해(재밍)하여, 비행 데이터에 미치는 영향을 분석하는 실험을 수행한다.**

### 실험 목표
**GPS 스푸핑**: 드론의 위치 정보를 변조하여 의도된 좌표로 이동하도록 유도  
**GPS 재밍**: 드론이 GPS 신호를 수신하지 못하게 하여 비행 제어 방해  
**MAVLink 데이터 비교**: 정상 비행과 공격 상황에서의 GPS 데이터 및 비행 패턴 변화 분석  

### 🔹 실험 방법
1 **기본 환경 설정**  
   - PX4 드론, QGroundControl(QGC), HackRF 세팅  
   - 정상적인 비행 경로 및 `tlog` 데이터 확보  

2 **GPS 스푸핑 신호 생성 및 송출**  
   - **GPS-SDR-SIM을 이용해 변조된 GPS 데이터 (`.bin → .c16`) 생성**  
   - HackRF로 **GPS L1(1575.42MHz) 대역에 스푸핑 신호 송출**  
   - 드론이 목표 좌표로 이동하는지 관찰  

3 **비행 로그 분석**  
   - **tlog(MAVLink 메시지)와 vehicle.csv 데이터 비교**  
   - **GPS 데이터 변조 성공 여부 및 비행 패턴 변화 분석**  
   - MAVLink 메시지 중 **GPS_RAW_INT, GLOBAL_POSITION_INT** 분석  

## ⚙️ Setup & Installation
### MAVLink 데이터 분석 환경 구축
```bash
# MAVExplorer 설치 (MAVLink 로그 분석 도구)
pip install pymavlink
pip install mavproxy
