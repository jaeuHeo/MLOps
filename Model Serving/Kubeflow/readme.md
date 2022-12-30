## kubeflow 설치
### docker 와 kubectl이 설치되었다는 가정하에 진행한다. kubectl은 v1.21.7 버전이다.
### 쿠버네티스 클러스터는 빠르고 간편하게 구축할 수 있는 Minikube로 세팅하였다. v1.24.0 버전이다.

### minikube 클러스터 구축 진행
minikube start --driver=docker   --kubernetes-version=v1.21.7   --extra-config=apiserver.service-account-signing-key-file=/var/lib/minikube/certs/sa.key   --extra-config=apiserver.service-account-issuer=kubernetes.default.svc --cpus=8 --memory=24g disk=100g

- driver = docker
- k8s version = v1.21.7
- cpu, ram, disk 사이즈는 각자 컴퓨팅 환경에 맞게 구성하면 된다.

https://github.com/kubeflow/manifests 의 readme의 커스터마이즈 build 순서대로 진행해도 되지만 나는 

while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done

의 반복문을 통해서 manifests 하위 폴더인 example의 kustomization.yaml을 읽어서 모두 설치하는 방식으로 진행했다.

만약, 모든 컴포넌트 설치를 원치않다면 하나하나 설치하는 것을 추천한다.

모두 빌드 됐다면,

![image](https://user-images.githubusercontent.com/96987794/210043354-5474fb48-e01e-466d-aff7-ab52cd00de69.png)

위와 같은 running pod 목록을 확인할 수 있다.

