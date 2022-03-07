# Jenkins practice

## 실패하는 구간 log 찍어서 해결하기
- 환경 변수 문제 등 찾아서 해결함
- 왜 빌드 과정이 엄청 느린건지 모르겠음
  - 구글링 해서 얻은 의심점은 docker image build 과정에서 내가 프리티어 EC2를 사용하고 있는데 cache가 너무 쌓인게 아닌가 싶음
  - 확인해서 우선 제거해보고 속도 체크해보려 함


### 확인 결과
- 프리티어 EC2 에서 docker image cache랑 build 과정 cache가 점점 쌓이던게 문제가 맞는 것 같다
- sudo docker image prune -a
- sudo docker container prune 등의 명령어로 사용하지 않는 걸 제거하던 중
- 퍼블릭 주소:8080 접속이 끊겼고, 재접속이 안된다
  - 그래도 느린 상태에서 마지막 테스트가 성공적으로 완성됐고, S3에 front file이 올라갔고, server build도 성공한 걸로 확인됐다
  - 이 부분에 대해서 docker image 를 관리하는 cron을 설정하거나 관리 방법을 공부해봐야 할 듯 하다
  - 그리고 개발 환경 서버에서도 jenkins를 같이 돌리는 건 안될 것 같고, docker로 올린다면 ECR에 push하고 trigger로 개발환경 서버나 prod 서버에서 받는 순으로 해야 할 것 같다
