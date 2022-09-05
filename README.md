# README

## Hashicorp 볼트란 무엇입니까?

Vault는 민감한 데이터를 보호하기 위해 안전한 비밀 관리를 제공하는 소프트웨어이며 이 기사에서는 금고 도커를 사용하여 비밀 엔진을 만들고 관리하는 방법을 보여줍니다. 비밀은 텍스트 속성 또는 데이터에서 토큰, 암호, X509 인증서 및 암호화, 인증 및 서명에 사용되는 대칭 및 비대칭 키에 이르기까지 모든 것이 될 수 있습니다 . Vault는 API 기반이며 표준 REST API 클라이언트 소프트웨어 또는 내장 CLI 도구 또는 Vault UI와 함께 사용할 수 있습니다.

## docker-compose.yml

1. 볼트 서버
   1. 최신 볼트 이미지를 사용합니다.
   2. 포트 8200 노출
   3. VAULT_ADDR 환경 변수를 서버 시작 시 권장 사항으로 설정하고 VAULT_DEV_ROOT_TOKEN_ID 환경 변수를 설정 하여 볼트 클라이언트 컨테이너에서 사용할 루트 토큰을 초기화합니다.
   4. 볼트가 메모리를 잠글 수 있도록 cap_add IPC_LOCK 을 설정합니다 .
   5. Vault-server를 로컬 네트워크에 추가합니다.
2. 볼트 클라이언트
   1. 동일한 디렉토리의 Dockerfile에서 빌드합니다. 이는 아래에서 자세히 설명합니다.
   2. Vault 서버와 통신하도록 VAULT_ADDR 환경 변수를 설정 합니다.
   3. Vault-client를 로컬 네트워크에 추가합니다.
3. 볼트-클라이언트 및 볼트-서버가 실행될 로컬 네트워크를 생성합니다.

## Dockerfile

- 위의 Dockerfile은 우분투 20.04 이미지에서 컨테이너를 만들고 볼트 클라이언트로 실행하는 데 필요한 종속성을 설치합니다. 그런 다음 Dockerfile은 아래에 자세히 설명된 run.sh의 COPY 및 CMD를 지시합니다.

## run.sh

1. 클라이언트는 볼트 상태 명령을 통해 서버가 시작될 때까지 기다립니다. 이 예에서는 5초면 충분합니다. 사용 사례에 따라 필요에 따라 조정합니다.
2. 비밀을 관리하기 전에 먼저 볼트에 인증해야 합니다. 이 예에서는 docker-compose.yml 파일의 vault-server 컨테이너에 대한 환경 변수로 설정된 루트 토큰을 사용하여 인증합니다. 이것은 단순성을 위해 개발 환경에서 사용되지만 프로덕션 또는 민감한 환경에서 실행하기 위한 것은 아닙니다.
3. 볼트 비밀 엔진을 초기화합니다.
4. 이전 단계에서 만든 비밀 엔진에 비밀을 추가합니다.

## REFERENCE

- [HashiCorp Vault Container로 실행](https://ikcoo.tistory.com/363)
- [볼트 도커 – Hashicorp Vault를 사용한 도커 작성 예제](https://www.misterpki.com/vault-docker/)
- [5가지 종류의 KMS(Key Management System)](https://blog.naver.com/PostView.nhn?blogId=aepkoreanet&logNo=220968117741&redirect=Dlog&widgetTypeCall=true&directAccess=false)
