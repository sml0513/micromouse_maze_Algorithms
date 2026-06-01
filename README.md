# Micromouse Maze Algorithms

농연/매연 환경에서 로봇이 안전지대까지 탈출 가능한지 검증하기 위한 CNN 기반 미로 인식 및 경로 탐색 알고리즘 실험 프로젝트입니다.

## 프로젝트 개요

이 프로젝트는 단순한 미로 탈출 알고리즘 구현을 넘어, 화재 현장의 농연 환경을 가정하여 로봇이 제한된 정보만으로 미로 구조를 인식하고 안전한 대피 경로를 찾을 수 있는지 검증하는 것을 목표로 합니다.

Google Colab 환경에서 CNN 모델을 학습하여 미로 또는 도면 정보를 확인하고, BFS, DFS, Dijkstra, A*, 좌수법, 우수법 등의 경로 탐색 알고리즘을 비교합니다. 또한 무작위 장애물이 추가된 상황에서도 탈출 가능한 경로를 찾을 수 있는지 시뮬레이션합니다.

## 주요 기능

* 미로 데이터 확인 및 전처리
* CNN 기반 미로/도면 인식
* 학습, 평가, 예측 과정 시각화
* BFS 경로 탐색
* DFS 경로 탐색
* Dijkstra 최단 경로 탐색
* A* 휴리스틱 기반 경로 탐색
* 좌수법, 우수법 기반 미로 탐색
* 무작위 장애물 추가 실험
* 알고리즘별 경로 길이, 방문 셀 수, 실행 시간 비교
* 미로 탈출 경로 시각화
* 연구보고서용 결과 그래프 및 결론 생성

## 연구 목적

본 연구의 목적은 농연으로 인해 시야 확보가 어려운 재난 환경에서 로봇이 미로 구조를 파악하고 안전지대까지 이동할 수 있는지 알고리즘적으로 검증하는 것입니다.

이를 위해 CNN을 이용해 미로 정보를 확인하고, 여러 경로 탐색 알고리즘의 성능을 비교하여 실제 재난 탐색 로봇에 적합한 경로 탐색 방식을 분석합니다.

## 사용 기술

* Python
* Google Colab
* NumPy
* Pandas
* Matplotlib
* TensorFlow / Keras
* 경로 탐색 알고리즘

## 알고리즘 비교 항목

| 항목     | 설명                    |
| ------ | --------------------- |
| 성공 여부  | 출발점에서 목표점까지 도달했는지 확인  |
| 경로 길이  | 최종 탈출 경로의 칸 수         |
| 방문 셀 수 | 알고리즘이 탐색한 셀의 개수       |
| 실행 시간  | 경로 탐색에 걸린 시간          |
| 시각화 결과 | 알고리즘별 탐색 과정과 최종 경로 표시 |

## 데이터 처리 흐름

1. GitHub의 micromouse maze 데이터를 Colab 런타임으로 불러옵니다.
2. 미로 배열을 16x16 입력 데이터로 변환하고 CNN 입력 형식에 맞게 정규화합니다.
3. 라벨을 정수 인코딩하고 학습/테스트 데이터로 분리합니다.
4. CNN 학습 로그를 epoch 단위 DataFrame으로 정리합니다.
5. CNN 평가 결과, 혼동 행렬, 오분류 샘플을 분석합니다.
6. BFS, DFS, Dijkstra, A*, 좌수법, 우수법의 탐색 결과를 동일한 지표로 비교합니다.
7. 매연, 일산화탄소, 온도, 화염 감지값을 위험도 점수로 변환하여 위험도 기반 경로 탐색 가능성을 확인합니다.
8. 결과 표는 `results/tables/`, 그래프는 `results/figures/`에 저장합니다.

## 데이터 품질 검증 항목

노트북에는 `validate_maze_dataset()` 함수가 추가되어 다음 항목을 자동으로 점검합니다.

* 미로 배열 크기 일관성
* 벽 정보 값 범위
* 시작점과 목표점 좌표 유효성
* 탈출 가능한 미로 여부
* 중복 미로 여부
* 라벨 누락 여부
* NaN 또는 비정상 값 여부

## CNN 평가 지표

CNN 모델 평가는 다음 지표를 DataFrame과 CSV로 저장합니다.

* Top-1 accuracy
* Top-5 accuracy
* error rate
* test sample count
* class count
* model name
* recognition mode
* confusion matrix 기반 오분류 클래스 쌍
* 클래스별 오류율

## 알고리즘 비교 지표

탐색 알고리즘 결과는 `results/tables/algorithm_comparison.csv`에 저장되며 다음 컬럼을 포함합니다.

* algorithm
* success
* path_length
* visited_cells
* execution_time_ms
* obstacle_count
* cumulative_risk
* risk_pass_count
* reroute_count
* selected_as_best

## 센서 데이터 연동 계획

ESP32 기반 실제 로봇 실험으로 확장할 수 있도록 센서 CSV 스키마를 정의했습니다.

| 컬럼 | 의미 |
| --- | --- |
| `time_ms` | 측정 시간 |
| `tof_mm` | ToF 거리 센서값 |
| `mq2_raw` | MQ-2 매연 센서 raw 값 |
| `mq7_raw` | MQ-7 일산화탄소 센서 raw 값 |
| `temperature` | 온도 |
| `humidity` | 습도 |
| `flame_detected` | 불꽃 감지 여부 |
| `robot_x` | 로봇 x 좌표 |
| `robot_y` | 로봇 y 좌표 |
| `robot_dir` | 로봇 방향 |

실제 센서 데이터가 없을 때는 `generate_mock_sensor_log()`로 안전한 가상 센서 로그를 생성하고, `sensor_log_to_risk_map()`으로 셀별 위험도 heatmap을 만들 수 있습니다.

## 생기부/연구보고서 강조 요소

이 프로젝트는 단순 알고리즘 실행이 아니라 데이터 기반 탐구 흐름을 포함합니다.

* 미로 데이터 수집 및 유효성 검증
* CNN 입력을 위한 데이터 정규화와 라벨 인코딩
* 학습/테스트 데이터 분포 분석
* 학습 로그, 평가 지표, 오분류 결과의 DataFrame화
* 알고리즘별 경로 길이, 방문 셀 수, 실행 시간 비교
* 센서값을 위험도 데이터로 변환하는 구조 설계
* 연구보고서용 결론과 생기부용 활동 요약 자동 생성

## AI 개발 목표

이 프로젝트의 AI 개발 목표는 농연/매연 환경에서 제한된 미로 정보를 CNN으로 인식하고, 센서 기반 위험도 데이터를 결합하여 안전 대피 경로를 산출하는 것입니다.

* 입력: 미로 이미지 또는 16x16 미로 배열
* 추가 입력: MQ-2, MQ-7, 온도, 습도, 불꽃 감지, ToF 거리 기반 센서 데이터
* 출력: 미로/도면 분류 결과, 예측 신뢰도, 탈출 가능성, 최종 경로, 로봇 이동 명령
* AI 모델 역할: 농연 환경에서 제한된 시각 정보를 보완하는 CNN 기반 환경 인식
* 탐색 알고리즘 역할: CNN 인식 결과와 위험도 맵을 이용한 안전 경로 산출

## CNN 모델 구조

노트북은 기존 CNN 모델을 유지하면서 비교용 모델 구조를 별도 함수로 추가합니다.

* `build_baseline_cnn()`: 단순 Conv-ReLU 기반 baseline CNN
* `build_improved_cnn()`: Batch Normalization과 Dropout을 포함한 개선 CNN
* `save_model_summary()`: 모델 구조와 파라미터 수를 `results/tables/model_summary.txt`에 저장
* `compare_cnn_models()`: 모델 파라미터 수, 학습 정확도, 테스트 정확도, 오류율, 과적합 여부를 표로 저장

## 데이터셋 구성

`create_dataset_metadata()` 함수는 실험마다 데이터셋 정보를 기록합니다.

* dataset_name
* dataset_version
* total_samples
* image_shape
* class_count
* train_count
* test_count
* preprocessing_method
* augmentation_used
* random_seed
* created_at

결과는 `results/tables/dataset_metadata.csv`에 저장됩니다.

## 학습 및 평가 방법

기존 CNN 학습 흐름은 유지하고, 다음 AI 평가 기능을 추가했습니다.

* 학습 곡선 진단: `training_diagnosis.csv`, `training_diagnosis.png`
* 모델 저장 및 재사용: `save_trained_model()`, `load_trained_model()`
* 학습 관리 설정: EarlyStopping, ModelCheckpoint, ReduceLROnPlateau에 해당하는 설정 저장
* 실험 설정 저장: `results/experiment_config.json`

## 하이퍼파라미터 실험

`run_hyperparameter_experiments()` 함수는 실행 시간이 길어지지 않도록 작은 실험으로 설정되어 있습니다.

비교 항목:

* learning_rate
* batch_size
* dropout_rate
* optimizer
* epoch
* test accuracy
* test loss
* error rate
* training time

결과는 `results/tables/hyperparameter_experiments.csv`에 저장됩니다.

## 오분류 분석

`analyze_misclassified_samples()`와 `analyze_prediction_confidence()` 함수는 CNN이 틀린 예측과 예측 신뢰도를 분석합니다.

* 오분류 샘플 개수
* 오분류율
* 실제 라벨과 예측 라벨 비교
* 예측 확률
* Top-1, Top-3, Top-5 예측
* 낮은 신뢰도 샘플 수

결과는 `results/tables/`와 `results/figures/`에 저장됩니다.

## 센서 데이터와 위험도 맵

센서값은 위험도 점수로 변환되어 경로 탐색에 활용됩니다.

* `prepare_sensor_risk_dataset()`: 센서 로그를 위험도 분류 데이터셋으로 변환
* `train_risk_predictor_baseline()`: mock 센서 데이터 기반 위험도 예측 baseline 학습
* `sensor_log_to_risk_map()`: 셀별 위험도 heatmap 생성

현재는 안전한 mock 센서 데이터를 사용하며, 실제 화재나 유독가스 실험은 수행하지 않습니다.

## AI 의사결정 파이프라인

`ai_decision_pipeline()` 함수는 CNN 예측 결과와 센서 위험도 맵을 결합해 최종 경로와 로봇 이동 명령을 생성합니다.

처리 흐름:

1. CNN으로 미로/도면 확인
2. 센서값으로 위험도 맵 생성
3. 위험도 반영 경로 탐색
4. 최종 경로 선택
5. 로봇 이동 명령 변환

## 향후 실제 로봇 연동 계획

향후 ESP32 기반 센서 로그를 CSV로 수집하고, CNN 추론 결과와 위험도 예측 모델을 실제 로봇 주행 의사결정에 연결할 계획입니다.

* ESP32: 센서값 수집 및 주행 제어
* ToF 센서: 벽 및 장애물 거리 측정
* MQ-2 / MQ-7: 매연 및 일산화탄소 위험도 감지
* DHT22: 온습도 측정
* 불꽃센서: 화염 감지
* Python/Colab: CNN 추론, 위험도 맵 생성, 경로 탐색, 결과 리포트 생성

## 실행 방법

1. `micromouse_maze_알고리즘.ipynb` 파일을 Google Colab에서 엽니다.
2. 위에서부터 순서대로 셀을 실행합니다.
3. CNN 학습 및 평가 결과를 확인합니다.
4. 각 미로 탈출 알고리즘의 결과를 비교합니다.
5. 시각화 그래프와 연구보고서용 결론을 확인합니다.

## 하드웨어 연동 계획

추후 BOM에 포함된 ESP32, ToF 거리 센서, MQ-2, MQ-7, DHT22, 불꽃센서, 모터 드라이버, DC 기어모터를 이용해 실제 로봇 구현과 연결할 계획입니다.

* ToF 센서: 벽 거리 측정
* MQ-2 / MQ-7: 매연 및 일산화탄소 위험도 감지
* DHT22: 온습도 측정
* 불꽃센서: 화염 감지
* ESP32: 센서값 수집 및 모터 제어
* Python/Colab: CNN 분석, 경로 탐색, 결과 시각화

## 최종 목표

최종적으로 이 프로젝트는 CNN 기반 환경 확인, 미로 탈출 알고리즘 비교, 장애물 상황에서의 경로 재탐색, 실제 로봇 하드웨어 연동까지 확장하여 농연 환경에서의 안전 대피 경로 산출 가능성을 검증하는 것을 목표로 합니다.
