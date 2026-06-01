# Micromouse Maze Algorithms

마이크로마우스 미로 알고리즘 노트북을 기반으로, 화재 대피 로봇 적용 가능성 연구 흐름을 추가한 프로젝트입니다.

## 주요 파일

- `notebooks/micromouse_maze_알고리즘.ipynb`
- `notebooks/micromouse_maze_알고리즘_20260531_2359.ipynb`
- `docs/research_summary.md`
- `hardware/BOM.md`

## 구현 내용

- 7x7 부분 관측 패치 기반 위치 후보 추정
- 가상 센서 CSV 입력 모드
- smoke, CO, temperature, obstacle 기반 `risk_map` 생성
- risk-aware A* 경로 탐색
- 이동마다 센서값을 읽고 지도/위험도를 갱신하는 동적 재탐색 루프
- 결과 지표 정리:
  - `path_length`
  - `computation_time`
  - `observed_risk_exposure`
  - `true_risk_exposure`
  - `replanning_count`
  - `blocked_event_count`
  - `success`

## 안전 범위

이 프로젝트는 실제 화재, 실제 유독가스, 실제 고온 실험을 수행하지 않습니다. 모든 위험도 값은 안전한 모의 센서값 또는 가상 CSV 입력을 사용합니다.

## Colab 실행

노트북을 Google Colab에서 열고 위에서 아래로 실행하면 됩니다.

기존 정적 애니메이션은 기본 비활성화되어 있습니다.

```python
RUN_LEGACY_ANIMATION = False
```

화재 대피 모의 실험은 기본 활성화되어 있습니다.

```python
RUN_FIRE_ESCAPE_EXPERIMENT = True
```

## 외부 데이터/도구

`micromouse_maze_tool/` 디렉터리는 별도 Git 저장소이므로 이 저장소에는 포함하지 않습니다. 노트북 내부 Colab 셀에서 다음 저장소를 clone하도록 구성되어 있습니다.

```text
https://github.com/micromouseonline/micromouse_maze_tool.git
```

