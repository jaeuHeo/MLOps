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

NAMESPACE                   NAME                                                         READY   STATUS    RESTARTS   AGE
auth                        dex-5ddf47d88d-28pzx                                         1/1     Running   1          19m
cert-manager                cert-manager-7dd5854bb4-z2s6l                                1/1     Running   0          19m
cert-manager                cert-manager-cainjector-64c949654c-rb66c                     1/1     Running   0          19m
cert-manager                cert-manager-webhook-6b57b9b886-v22tr                        1/1     Running   0          19m
istio-system                authservice-0                                                1/1     Running   0          19m
istio-system                cluster-local-gateway-75cb7c6c88-8n9m6                       1/1     Running   0          19m
istio-system                istio-ingressgateway-79b665c95-rnl9n                         1/1     Running   0          19m
istio-system                istiod-86457659bb-2xzqs                                      1/1     Running   0          19m
knative-eventing            eventing-controller-79895f9c56-mk4tb                         1/1     Running   0          19m
knative-eventing            eventing-webhook-78f897666-96fk2                             1/1     Running   0          19m
knative-eventing            imc-controller-688df5bdb4-2zct2                              1/1     Running   0          19m
knative-eventing            imc-dispatcher-646978d797-442cc                              1/1     Running   0          19m
knative-eventing            mt-broker-controller-67c977497-bnrrs                         1/1     Running   0          19m
knative-eventing            mt-broker-filter-66d4d77c8b-cbj2s                            1/1     Running   0          19m
knative-eventing            mt-broker-ingress-5c8dc4b5d7-8bctr                           1/1     Running   0          19m
knative-serving             activator-7476cc56d4-wwnqn                                   2/2     Running   1          19m
knative-serving             autoscaler-5c648f7465-cvznw                                  2/2     Running   0          19m
knative-serving             controller-57c545cbfb-7g96q                                  2/2     Running   1          19m
knative-serving             istio-webhook-578b6b7654-xdfm9                               2/2     Running   1          19m
knative-serving             networking-istio-6b88f745c-qmrb5                             2/2     Running   1          19m
knative-serving             webhook-6fffdc4d78-fk444                                     2/2     Running   1          19m
kube-system                 coredns-558bd4d5db-rtxkr                                     1/1     Running   0          20m
kube-system                 etcd-minikube                                                1/1     Running   0          20m
kube-system                 kube-apiserver-minikube                                      1/1     Running   0          20m
kube-system                 kube-controller-manager-minikube                             1/1     Running   0          20m
kube-system                 kube-proxy-9bjgn                                             1/1     Running   0          20m
kube-system                 kube-scheduler-minikube                                      1/1     Running   0          20m
kube-system                 storage-provisioner                                          1/1     Running   0          20m
kubeflow-user-example-com   ml-pipeline-ui-artifact-5dd95d555b-lq9tb                     2/2     Running   0          14m
kubeflow-user-example-com   ml-pipeline-visualizationserver-6b44c6759f-jjwpr             2/2     Running   0          14m
kubeflow                    admission-webhook-deployment-667bd68d94-zc4x7                1/1     Running   0          19m
kubeflow                    cache-deployer-deployment-79fdf9c5c9-rgg8f                   2/2     Running   1          19m
kubeflow                    cache-server-6566dc7dbf-7rtkg                                2/2     Running   0          19m
kubeflow                    centraldashboard-8fc7d8cc-2jrll                              1/1     Running   0          19m
kubeflow                    jupyter-web-app-deployment-84c459d4cd-67kb4                  1/1     Running   0          18m
kubeflow                    katib-controller-68c47fbf8b-vqzsq                            1/1     Running   0          19m
kubeflow                    katib-db-manager-6c948b6b76-tr2gh                            1/1     Running   0          19m
kubeflow                    katib-mysql-7894994f88-ltkfc                                 1/1     Running   0          19m
kubeflow                    katib-ui-64bb96d5bf-sm56b                                    1/1     Running   0          19m
kubeflow                    kfserving-controller-manager-0                               2/2     Running   0          19m
kubeflow                    kfserving-models-web-app-5d6cd6b5dd-jlbbf                    2/2     Running   0          19m
kubeflow                    kubeflow-pipelines-profile-controller-69596b78cc-2862v       1/1     Running   0          19m
kubeflow                    metacontroller-0                                             1/1     Running   0          19m
kubeflow                    metadata-envoy-deployment-5b4856dd5-h8pjj                    1/1     Running   0          19m
kubeflow                    metadata-grpc-deployment-6b5685488-ndrs6                     2/2     Running   1          19m
kubeflow                    metadata-writer-548bd879bb-62kld                             2/2     Running   0          19m
kubeflow                    minio-5b65df66c9-cxr4v                                       2/2     Running   0          19m
kubeflow                    ml-pipeline-847f9d7f78-2b84l                                 2/2     Running   1          19m
kubeflow                    ml-pipeline-persistenceagent-d6bdc77bd-fsbjh                 2/2     Running   0          18m
kubeflow                    ml-pipeline-scheduledworkflow-5db54d75c5-ggkvm               2/2     Running   0          18m
kubeflow                    ml-pipeline-ui-5bd8d6dc84-hk4z4                              2/2     Running   0          18m
kubeflow                    ml-pipeline-viewer-crd-68fb5f4d58-w5d4v                      2/2     Running   1          18m
kubeflow                    ml-pipeline-visualizationserver-8476b5c645-jc8pk             2/2     Running   0          19m
kubeflow                    mpi-operator-5c55d6cb8f-dmvsx                                1/1     Running   0          19m
kubeflow                    mysql-f7b9b7dd4-nlrvk                                        2/2     Running   0          19m
kubeflow                    notebook-controller-deployment-6b75d45f48-6dp4v              1/1     Running   0          19m
kubeflow                    profiles-deployment-58d7c94845-7xlx7                         2/2     Running   0          19m
kubeflow                    tensorboard-controller-controller-manager-775777c4c5-tzc8v   3/3     Running   1          19m
kubeflow                    tensorboards-web-app-deployment-6ff79b7f44-9bwks             1/1     Running   0          19m
kubeflow                    training-operator-7d98f9dd88-f96tp                           1/1     Running   0          19m
kubeflow                    volumes-web-app-deployment-8589d664cc-kff9p                  1/1     Running   0          19m
kubeflow                    workflow-controller-5cbbb49bd8-gx8tj                         2/2     Running   2          19m

위와 같은 running pod 목록을 확인할 수 있다.

