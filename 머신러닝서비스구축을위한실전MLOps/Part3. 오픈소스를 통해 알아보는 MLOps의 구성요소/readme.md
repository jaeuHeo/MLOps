# Data Management
## 필요성
### 시계열 데이터
- 일정기간의 moving Avg 적용하는 경우가 상당히 많음 ex> 1 hour, 3 hour, 1 week, 1month...)
- 각 moving Avg 적용한 시계열 데이터들을 디렉토리, 파일명으로 구분해서 저장해도 좋지만 더 효율적인 무료 툴을 쓰면 더 좋다.

### 버전 관리
- GIT은 형상관리 툴이며, GitHub, GitLab 등은 GIT을 서버로 관리해서 UI, API 와 같은 사용자들이 사용할 수 있도록 해주는 GIT 호스팅 서버이다.
- GIT은 소스코드 버전관리에 용이하며, 데이터나 모델같이 크기가 큰 파일들은 S3에 저장하는 것이 일반적이다.
- 따라서, 소스코드와 데이터 모두 버전 관리할 수 있도록 도와주는 툴이 등장했다.

### DVC (Data Version Control)
- 대부분의 스토리지와 호환 (AMAZON S3, GOOGLE DRIVE...)
- 대부분의 GIT 호스팅서버와 연동
- 데이터뿐만 아니라 소스코드(전처리, 후처리 코드)를 포함한 Data pipeline을 작성할 수 있고, 재현가능하도록 DAG로 관리할 수 있음
- GIT과 유사한 인터페이스
- 저장 방식
  - 데이터는 실제로 Object Storage에 저장되고 GIT server에 데이터의 메타데이터만 저장된다.

# Model Management
## 필요성
- 머신러닝 Life Cycle 관리의 어려움
  - 모델 소스코드, Evaluation Metric 결과, 사용한 parameters, model.pkl 파일, 학습에 사용한 data, 데이터 전처리용 코드, 전처리된 data, 등등
  - 비슷한 작업의 반복, dependency 패키지들 버전관리, 사람 dependency, 테스트 어려움, reproduce되지않음, 모델 학습용 코드 개발자와 서빙용 코드 개발자가 분리되어있음

- 모델 코드, Metric, hyper parameter, 모델 파일, 학습 데이터, 전처리 코드 등을 기록하며 관리함으로 인해 데이터과학자 및 머신러닝 연구자들은 생성, 구성, 실험, 추적, 배포 등의 머신러닝 life cycle을 보다 쉽게 관리할 수 있음

### Tools
<img width="500" alt="image" src="https://user-images.githubusercontent.com/96987794/206476975-c5c98783-45bf-4b93-aae0-5efff71f6f88.png">

Tracking 이란?
머신러닝의 과정과 결과를 곳곳에서 기록한다는 의미로 "Tracking" 이라는 표현을 사용한다.
Tracking은 실험(Experiment)의 각 실행(Run)에서 일어나고, 구체적으로는 다음 내용들을 기록한다. 
코드 버전, 시작 및 종료 시간, 소스, 매개 변수, 메트릭, 아티팩트

### MLflow
- Experiment Management & Tracking
  - 머신러닝 관련 실험을 정의하고 실험들을 관리하고, 실행하고 각 실험의 내용들을 기록할 수 있음. ex) code, result, data, configs
- Projects
  - 모델의 의존성이 있는 모든 정보(데이터타입, 파라미터, 도커 정보, 파이토치 버전, 텐서플로우 버전 등)를 함께 넣어서 코드를 패키징
- Models
  - 모델이 어떤 프레임워크로 개발되었던 간에 통일된 형태로 배포해서 사용할 수 있도록 포맷화. ex) R ,pytorch, tensorflow 에 관련없이 동일하게 사용할수있도록 해줌
- Model Registry (최근에 추가)
  - MLflow로 실험했던 모델을 저장하고 관리하는 저장소. Production stage, Staging stage 와 같이 표시할 수 있고, 간편하게 서빙할 수 있음. MLflow CLI를 통해 손쉽게 배포할 수 있고 Python API도 제공

### MLflow 장점
- pip로 쉽게 설치가능 하며 마이그레이션이 쉬움
- 대시보드 UI 제공
- 다양한 client api 제공 (python, Java, R 등)
- 다양한 ml 프레임워크 지원 (scikit-learn, onnx, pytorch, keras, tensorflow 등)
- 다양한 백엔드 스토리지 연동 지원 (mysql, sqlite, mariadb, oracle 등)
- 다양한 artifact 스토리지 연동 지원 (Amazon S3, Google Cloud Storage, Azure Blob Storage 등)

### MLflow Tracking Server
- MLflow 는 Tracking 역할을 위한 별도의 서버를 제공한다. 이를 Tracking Server라고 부른다.
- mlflow.log_params, mlflow.log_metrics 등을 통해서 ./mlruns 에 바로 기록물을 저장했다면, 이제는 이 백엔드 서버를 통해서 저장하게 된다.
<img width="352" alt="image" src="https://user-images.githubusercontent.com/96987794/206486294-b3db9c42-69f2-4f43-9118-713eacd6343b.png">

