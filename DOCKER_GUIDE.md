# Docker를 이용한 Tavily MCP 서버 실행 가이드

이 문서는 Docker를 사용하여 `tavily-mcp` 서버를 컨테이너 환경에서 실행하는 방법을 안내합니다.

Docker를 사용하면 Node.js와 같은 런타임을 로컬에 직접 설치할 필요 없이, 격리되고 일관된 환경에서 애플리케이션을 실행할 수 있습니다.

## 사전 준비

1.  **Docker 설치**: 시스템에 [Docker](https://www.docker.com/get-started)가 설치되어 있어야 합니다.
2.  **.env 파일 생성**: 프로젝트의 루트 디렉터리에 `.env` 파일을 생성하고 Tavily API 키를 추가해야 합니다.

    ```
    # .env
    TAVILY_API_KEY=your-tavily-api-key-here
    ```

## 실행 방법

이 애플리케이션은 웹 서버가 아니며, 표준 입출력(stdio)을 통해 클라이언트와 통신합니다. 따라서 컨테이너를 실행할 때 표준 입출력을 연결해 주는 방식(`docker run -i` 또는 `docker-compose run`)을 사용해야 합니다.

### 방법 1: `docker run`으로 직접 실행하기

이 방법은 Docker 이미지를 직접 빌드하고 실행하여 컨테이너를 시작합니다.

1.  **Docker 이미지 빌드**
    프로젝트 루트 디렉터리에서 아래 명령어를 실행하여 `tavily-mcp-image`라는 이름의 이미지를 빌드합니다.
    ```bash
    docker build -t tavily-mcp-image .
    ```

2.  **`docker run`으로 컨테이너 실행**
    VS Code, Claude Desktop 등 MCP 클라이언트의 서버 실행 설정에 아래 명령어를 등록합니다.
    ```bash
    docker run -i --rm --env-file ./.env tavily-mcp-image
    ```
    -   `-i` (`--interactive`): 컨테이너의 표준 입력(stdin)을 활성화하여 클라이언트와 통신할 수 있도록 합니다. **(필수)**
    -   `--rm`: 컨테이너 실행이 종료되면 자동으로 삭제하여 불필요한 리소스가 남지 않도록 합니다.
    -   `--env-file ./.env`: API 키가 포함된 `.env` 파일을 컨테이너 환경 변수로 전달합니다.

### 방법 2: `docker-compose run`으로 실행하기

`docker-compose`를 사용하면 여러 컨테이너 설정을 `compose.yaml` 파일로 쉽게 관리할 수 있습니다. `docker-compose up`이 아닌 `docker-compose run`을 사용하여 stdio 기반의 일회성 명령을 실행합니다.

1.  **`compose.yaml` 파일 확인**
    프로젝트의 `compose.yaml` 파일이 아래와 같이 `build` 컨텍스트를 포함하고 `ports` 설정이 없는지 확인합니다.
    ```yaml
    services:
      app:
        build: .
        image: kojeomstudio/tavily-mcp:v0
        container_name: tavily-mcp-app
        env_file:
          - ./.env
        restart: unless-stopped
    ```

2.  **`docker-compose run`으로 서비스 실행**
    MCP 클라이언트의 서버 실행 설정에 아래 명령어를 등록합니다.
    ```bash
    docker-compose run --rm app
    ```
    -   이 명령어는 `compose.yaml`에 정의된 `app` 서비스를 일회성으로 실행하고, 현재 터미널의 입출력을 컨테이너의 입출력에 연결합니다.
    -   `--rm` 플래그는 실행 종료 후 컨테이너를 자동으로 삭제합니다.
