# LAIDD 실습 Docker 이미지

LAIDD 실습 모델을 로컬에서 실행하기 위한 Docker 이미지의 빌드 정의입니다.
각 Dockerfile은 공개 저장소의 고정된 커밋과 모델 가중치를 받아 빌드 컨텍스트 없이 단독으로 빌드됩니다.

| 모델 | 이미지 | GPU | 라이선스 | Dockerfile |
|---|---|---|---|---|
| ABodyBuilder3 | `ghcr.io/connects-scv/laidd-abodybuilder3:latest` | 필요 | Apache-2.0 | [Dockerfile_ab3](dockerfiles/Dockerfile_ab3) |
| ADMET-AI | `ghcr.io/connects-scv/laidd-admet-ai:latest` | - | MIT | [Dockerfile_admet_ai](dockerfiles/Dockerfile_admet_ai) |
| AnthroAb | `ghcr.io/connects-scv/laidd-anthroab:latest` | - | MIT | [Dockerfile_anthroab](dockerfiles/Dockerfile_anthroab) |
| DiffDock | `ghcr.io/connects-scv/laidd-diffdock:latest` | 필요 | MIT | [Dockerfile_diffdock](dockerfiles/Dockerfile_diffdock) |
| FlowDock | `ghcr.io/connects-scv/laidd-flowdock:latest` | 필요 | MIT | [Dockerfile_flowdock](dockerfiles/Dockerfile_flowdock) |
| GNINA | `ghcr.io/connects-scv/laidd-gnina:latest` | 필요 | Apache-2.0 | [Dockerfile_gnina](dockerfiles/Dockerfile_gnina) |
| Humatch | `ghcr.io/connects-scv/laidd-humatch:latest` | - | BSD-3-Clause | [Dockerfile_humatch](dockerfiles/Dockerfile_humatch) |
| ImmuScope | `ghcr.io/connects-scv/laidd-immuscope:latest` | 필요 | GPL-3.0 | [Dockerfile_immuscope](dockerfiles/Dockerfile_immuscope) |
| LigTMap | `ghcr.io/connects-scv/laidd-ligtmap:latest` | - | BSD-3-Clause | [Dockerfile_ligtmap](dockerfiles/Dockerfile_ligtmap) |
| Lip | `ghcr.io/connects-scv/laidd-lip:latest` | 필요 | Apache-2.0 | [Dockerfile_lip](dockerfiles/Dockerfile_lip) |
| MHCflurry | `ghcr.io/connects-scv/laidd-mhcflurry:latest` | 필요 | Apache-2.0 | [Dockerfile_mhcflurry](dockerfiles/Dockerfile_mhcflurry) |
| OpenFold3 | `ghcr.io/connects-scv/laidd-openfold3:latest` | 필요 | Apache-2.0 | [Dockerfile_openfold3](dockerfiles/Dockerfile_openfold3) |
| ProteinMPNN | `ghcr.io/connects-scv/laidd-proteinmpnn:latest` | 필요 | MIT | [Dockerfile_proteinmpnn](dockerfiles/Dockerfile_proteinmpnn) |
| Protenix | `ghcr.io/connects-scv/laidd-protenix:latest` | 필요 | Apache-2.0 | [Dockerfile_protenix](dockerfiles/Dockerfile_protenix) |
| RFdiffusion3 | `ghcr.io/connects-scv/laidd-rfdiffusion-v3:latest` | 필요 | BSD-3-Clause | [Dockerfile_rfd3](dockerfiles/Dockerfile_rfd3) |

모든 이미지는 `linux/amd64` 입니다. GPU 이미지는 호스트에 NVIDIA 드라이버와 NVIDIA Container Toolkit이 필요합니다.

## 실행

```bash
mkdir -p input output        # 입력 파일을 ./input 에 둡니다
docker run --rm [--gpus all] \
  -v "$PWD/input:/work/input" -v "$PWD/output:/work/output" \
  ghcr.io/connects-scv/laidd-<모델>:latest <모델별 실행 인자>
```

입력은 컨테이너의 `/work/input`, 결과는 `/work/output` 입니다. 이미지 이름 뒤에 인자를 주지 않으면 환경 점검(셀프체크)만 하고 끝납니다.
모델별 실행 인자와 입력 형식은 LAIDD Lab 각 모델 페이지의 「실행 가이드 내려받기 (.md)」에 있습니다.

## 빌드

`dockerfiles/` 가 main에 푸시되면 GitHub Actions가 15개를 모두 빌드해 GHCR에 올립니다.
일부만 빌드하려면 Actions → *Build and push practice Docker images* → Run workflow 에서 `only` 에 슬러그를 공백으로 구분해 넣습니다(예: `gnina lip`).

로컬 빌드:

```bash
docker build --platform linux/amd64 -t laidd-gnina - < dockerfiles/Dockerfile_gnina
```

## 유지보수

Dockerfile에서 설치 경로·CLI·가중치 위치를 바꾸면, LAIDD-Lab 저장소 `utils/dockerGuide.ts` 의 `DOCKER_RUN`(모델별 실행 인자)도 함께 확인해야 합니다.
