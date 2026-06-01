# 연구 요약

이 프로젝트는 기존 마이크로마우스 미로 탐색 코드를 화재 대피 로봇 적용 가능성 연구에 맞게 확장한 시뮬레이션입니다.

## 핵심 변경점

- 전체 미로 지도 의존도를 줄이기 위해 7x7 부분 관측 기반 위치 후보 추정 함수를 추가했습니다.
- smoke, CO, temperature, obstacle 값을 기반으로 위험도 지도를 생성합니다.
- 기존 Dijkstra/A* 비교 흐름에 risk-aware A*를 추가했습니다.
- 로봇이 한 칸 이동할 때마다 센서값을 읽고, 알려진 지도와 위험도를 갱신하며, 경로가 막히면 재탐색합니다.
- 실제 센서가 없어도 `VirtualSensorCSV`와 synthetic 모드로 실행할 수 있습니다.

## 평가 지표

- `path_length`
- `computation_time`
- `observed_risk_exposure`
- `true_risk_exposure`
- `replanning_count`
- `blocked_event_count`
- `success`

## 실험 범위

본 검증은 실제 화재나 유독가스 실험이 아니라 안전한 모의 위험도 값 기반 시뮬레이션입니다.

