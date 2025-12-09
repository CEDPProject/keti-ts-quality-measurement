# KETI Time Series Quality Measurement

![Python](https://img.shields.io/badge/python-3.7+-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

시계열 데이터의 품질을 종합적으로 측정하고 평가하는 Python 라이브러리입니다. 구문적 정확도, 의미적 정확도, 범위 정확도, 부정확도 위험도 등 다양한 품질 메트릭을 제공합니다.

## ✨ 주요 기능

- **구문적 정확도 (Syntactic Accuracy)**: 데이터 타입, 형식, 구조적 무결성 검증
- **의미적 정확도 (Semantic Accuracy)**: 비즈니스 규칙 및 논리적 일관성 검증  
- **범위 정확도 (Range Accuracy)**: 허용 범위 및 도메인 제약 조건 검증
- **부정확도 위험도 (Inaccuracy Risk)**: 통계적 이상치 및 품질 위험도 분석
- **종합 품질 점수**: 다차원 메트릭을 종합한 통합 품질 평가
- **시각화 도구**: 품질 메트릭의 직관적인 시각적 표현

## 📦 설치

### 요구 사항
- Python 3.7 이상
- pandas >= 1.0.0
- numpy >= 1.18.0
- matplotlib >= 3.0.0
- seaborn >= 0.11.0

### 설치 방법

```bash
# 저장소 클론
git clone https://github.com/your-org/keti-ts-quality-measurement.git
cd keti-ts-quality-measurement

# 의존성 설치
pip install -r requirements.txt
```

## 🚀 빠른 시작

### 기본 사용법

```python
import pandas as pd
from quality.measurement.combined_quality_metrics import CombinedQualityMetrics
from quality.visualization.visualisation_data_quality import DataQualityVisualization

# 샘플 데이터 로드
df = pd.read_csv("test.csv")

# 범위 제한 및 예상 타입 정의
range_limits = {
    'temperature': (-50, 100),
    'humidity': (0, 100),
    'pressure': (900, 1100)
}

expected_types = {
    'temperature': 'float64',
    'humidity': 'float64', 
    'pressure': 'float64'
}

# 품질 메트릭 계산
quality_metrics = CombinedQualityMetrics(
    df=df,
    range_limits=range_limits,
    expected_types=expected_types,
    z_threshold=3.0,
    percentile_range=(5, 95)
)

# 종합 품질 평가 결과 조회
results = quality_metrics.get_combined_metrics()
print("Overall Quality Score:", results['overall_quality_metrics']['overall_quality_score'])
```

### 품질 메트릭 시각화

```python
# 품질 메트릭 시각화
visualizer = DataQualityVisualization(df, range_limits, expected_types)

# 종합 품질 대시보드 생성
visualizer.plot_combined_quality_dashboard()

# 개별 품질 메트릭 시각화
visualizer.plot_syntactic_accuracy()
visualizer.plot_semantic_accuracy()
visualizer.plot_range_accuracy()
visualizer.plot_inaccuracy_risk()
```

## 📊 품질 메트릭 상세

### 1. 구문적 정확도 (Syntactic Accuracy)
데이터의 구조적 무결성과 형식 준수를 평가합니다.

- **데이터 타입 일치도**: 예상 타입과 실제 타입의 일치 여부
- **누락 값 비율**: 결측치의 비율과 분포
- **중복 값 검출**: 중복 레코드의 식별 및 비율

### 2. 의미적 정확도 (Semantic Accuracy)  
비즈니스 규칙 및 논리적 일관성을 검증합니다.

- **논리적 일관성**: 컬럼 간 논리적 관계 검증
- **참조 무결성**: 외래키 및 관계 데이터 일관성
- **비즈니스 규칙 준수**: 도메인별 비즈니스 로직 검증

### 3. 범위 정확도 (Range Accuracy)
허용 가능한 값의 범위 및 도메인 제약 조건을 검증합니다.

- **허용 범위 검증**: 최소/최대 값 범위 준수 여부
- **도메인 제약 조건**: 허용된 값 집합 내 포함 여부
- **경계 값 분석**: 경계값 근처의 데이터 분포 분석

### 4. 부정확도 위험도 (Inaccuracy Risk)
통계적 이상치 및 품질 위험 요소를 분석합니다.

- **통계적 이상치**: Z-score 및 IQR 기반 이상치 탐지
- **분포 일관성**: 예상 분포와 실제 분포의 차이
- **변화점 탐지**: 시계열 패턴 변화점 식별

## 🏗️ 아키텍처

```
quality/
├── measurement/
│   ├── data_quality_metrices.py      # 핵심 품질 메트릭 계산
│   ├── overall_quality_metrics.py    # 전체 품질 평가
│   ├── feature_quality_metrics.py    # 피처별 품질 분석
│   └── combined_quality_metrics.py   # 종합 품질 메트릭
└── visualization/
    └── visualisation_data_quality.py # 품질 메트릭 시각화
```

### 핵심 클래스

#### CombinedQualityMetrics
전체 품질 메트릭과 피처별 품질 메트릭을 통합하여 종합적인 품질 평가를 제공합니다.

```python
class CombinedQualityMetrics:
    def __init__(self, df, range_limits=None, expected_types=None, 
                 error_values=None, z_threshold=None, percentile_range=None)
    def get_combined_metrics(self) -> dict
```

#### DataQualityMetrics  
구문적 정확도, 의미적 정확도, 범위 정확도, 부정확도 위험도의 핵심 계산을 담당합니다.

```python
class DataQualityMetrics:
    def calculate_syntactic_accuracy(self) -> float
    def calculate_semantic_accuracy(self) -> float
    def calculate_range_accuracy(self) -> float
    def calculate_inaccuracy_risk(self) -> float
```

## 📈 사용 사례

### 1. 실시간 데이터 품질 모니터링

```python
import schedule
import time

def monitor_data_quality():
    # 실시간 데이터 수집
    current_data = fetch_realtime_data()
    
    # 품질 평가
    quality_metrics = CombinedQualityMetrics(current_data, range_limits, expected_types)
    results = quality_metrics.get_combined_metrics()
    
    # 임계치 기반 알림
    if results['overall_quality_metrics']['overall_quality_score'] < 0.8:
        send_quality_alert(results)

# 매시간 품질 모니터링 실행
schedule.every().hour.do(monitor_data_quality)
```

### 2. 배치 데이터 품질 보고서

```python
def generate_quality_report(file_paths):
    quality_results = []
    
    for file_path in file_paths:
        df = pd.read_csv(file_path)
        quality_metrics = CombinedQualityMetrics(df, range_limits, expected_types)
        results = quality_metrics.get_combined_metrics()
        
        quality_results.append({
            'file': file_path,
            'timestamp': pd.Timestamp.now(),
            'quality_score': results['overall_quality_metrics']['overall_quality_score'],
            'syntactic_accuracy': results['data_quality_metrics']['syntactic_accuracy'],
            'semantic_accuracy': results['data_quality_metrics']['semantic_accuracy']
        })
    
    return pd.DataFrame(quality_results)
```

### 3. 데이터 전처리 파이프라인 통합

```python
def quality_aware_preprocessing(df):
    # 1. 초기 품질 평가
    initial_quality = CombinedQualityMetrics(df, range_limits, expected_types)
    initial_score = initial_quality.get_combined_metrics()
    
    # 2. 품질 기반 전처리 전략 결정
    if initial_score['data_quality_metrics']['syntactic_accuracy'] < 0.9:
        df = apply_data_cleaning(df)
    
    if initial_score['data_quality_metrics']['range_accuracy'] < 0.8:
        df = apply_outlier_treatment(df)
    
    # 3. 최종 품질 검증
    final_quality = CombinedQualityMetrics(df, range_limits, expected_types)
    return df, final_quality.get_combined_metrics()
```

## 🔧 고급 설정

### 사용자 정의 품질 임계값

```python
# 사용자 정의 Z-score 임계값 및 백분위수 범위
quality_metrics = CombinedQualityMetrics(
    df=df,
    range_limits=range_limits,
    expected_types=expected_types,
    z_threshold=2.5,  # 더 엄격한 이상치 탐지
    percentile_range=(10, 90)  # 10-90% 백분위수 범위
)
```

### 오류 값 패턴 정의

```python
# 특정 오류 값 패턴 정의
error_values = [-9999, 'N/A', 'NULL', '']

quality_metrics = CombinedQualityMetrics(
    df=df,
    range_limits=range_limits,
    expected_types=expected_types,
    error_values=error_values
)
```

## 📝 테스트

```bash
# 품질 테스트 실행
python quality_test.py

# 예상 출력
Overall Quality Score: 0.85
Syntactic Accuracy: 0.92
Semantic Accuracy: 0.88
Range Accuracy: 0.79
Inaccuracy Risk: 0.81
```


## 🔗 관련 프로젝트

- [keti-ts-dataset-search](../keti-ts-dataset-search): 시계열 데이터셋 유사성 검색
- [keti-ts-preprocessing](../keti-ts-preprocessing): 모듈형 시계열 전처리 파이프라인
- [keti-datamanager](../keti-datamanager): 통합 시계열 데이터 관리 플랫폼

## 📞 지원

문제가 발생하거나 질문이 있으시면 [Issues](../../issues)를 통해 문의해 주세요.

---

**KETI (Korea Electronics Technology Institute)**  
시계열 데이터 품질 측정 및 관리를 위한 오픈소스 솔루션


---

## 📄 Contributor License Agreement (CLA)

본 프로젝트는 기여(Contribution)를 받기 위해 **Contributor License Agreement(CLA)** 서명을 요구합니다.

Pull Request를 제출하면 CLA Assistant가 자동으로 서명 여부를 확인하며,  
최초 1회 온라인 동의를 완료해야 기여가 포함될 수 있습니다.

🔗 **CLA 전문 보기:** [KETI Contributor License Agreement](https://gist.github.com/jwmoonP/79d622dc2ac2dedfbb2bf82396fbafd9)

---

## 📝 License

본 프로젝트는 **Apache License 2.0**을 따릅니다.  
자세한 내용은 [`LICENSE`](./LICENSE) 파일을 참고하세요.