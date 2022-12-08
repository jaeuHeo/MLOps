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
<img width="1200" alt="image" src="https://user-images.githubusercontent.com/96987794/206476975-c5c98783-45bf-4b93-aae0-5efff71f6f88.png">
