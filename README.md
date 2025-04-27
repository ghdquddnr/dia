<p align="center">
<a href="https://github.com/nari-labs/dia">
<img src="./dia/static/images/banner.png">
</a>
</p>
<p align="center">
<a href="https://tally.so/r/meokbo" target="_blank"><img alt="Static Badge" src="https://img.shields.io/badge/Join-Waitlist-white?style=for-the-badge"></a>
<a href="https://discord.gg/yBrqQ9Dd" target="_blank"><img src="https://img.shields.io/badge/Discord-Join%20Chat-7289DA?logo=discord&style=for-the-badge"></a>
<a href="https://github.com/nari-labs/dia/blob/main/LICENSE" target="_blank"><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg?style=for-the-badge" alt="LICENSE"></a>
</p>
<p align="center">
<a href="https://huggingface.co/nari-labs/Dia-1.6B"><img src="https://huggingface.co/datasets/huggingface/badges/resolve/main/model-on-hf-lg-dark.svg" alt="Dataset on HuggingFace" height=42 ></a>
<a href="https://huggingface.co/spaces/nari-labs/Dia-1.6B"><img src="https://huggingface.co/datasets/huggingface/badges/resolve/main/open-in-hf-spaces-lg-dark.svg" alt="Space on HuggingFace" height=38></a>
</p>

Dia는 Nari Labs에서 개발한 1.6B 파라미터의 텍스트-음성 변환 모델입니다.

Dia는 **대본에서 직접 매우 사실적인 대화를 생성**합니다. 오디오를 기반으로 출력을 조절할 수 있어 감정과 톤을 제어할 수 있습니다. 또한 웃음, 기침, 목청을 가다듬는 소리 등 비언어적 의사소통도 생성할 수 있습니다.

연구를 가속화하기 위해 사전 학습된 모델 체크포인트와 추론 코드를 제공하고 있습니다. 모델 가중치는 [Hugging Face](https://huggingface.co/nari-labs/Dia-1.6B)에서 호스팅됩니다. 현재 모델은 영어 생성만 지원합니다.

또한 [ElevenLabs Studio](https://elevenlabs.io/studio)와 [Sesame CSM-1B](https://github.com/SesameAILabs/csm)와 비교한 [데모 페이지](https://yummy-fir-7a4.notion.site/dia)도 제공합니다.

- (업데이트) ZeroGPU Space가 실행 중입니다! [여기](https://huggingface.co/spaces/nari-labs/Dia-1.6B)에서 지금 바로 시도해보세요. HF 팀의 지원에 감사드립니다 :)
- 커뮤니티 지원과 새로운 기능에 대한 접근을 위해 [디스코드 서버](https://discord.gg/yBrqQ9Dd)에 참여하세요.
- 더 큰 버전의 Dia로 놀아보세요: 재미있는 대화를 생성하고, 콘텐츠를 리믹스하고, 친구들과 공유하세요. 🔮 조기 접근을 위해 [대기자 명단](https://tally.so/r/meokbo)에 참여하세요.

## ⚡️ 빠른 시작

### pip를 통한 설치

```bash
# GitHub에서 직접 설치
pip install git+https://github.com/nari-labs/dia.git
```

### Gradio UI 실행

이렇게 하면 작업할 수 있는 Gradio UI가 열립니다.

```bash
git clone https://github.com/nari-labs/dia.git
cd dia && uv run app.py
```

또는 `uv`가 미리 설치되어 있지 않은 경우:

```bash
git clone https://github.com/nari-labs/dia.git
cd dia
python -m venv .venv
source .venv/bin/activate
pip install -e .
python app.py
```

모델은 특정 음성으로 미세 조정되지 않았습니다. 따라서 모델을 실행할 때마다 다른 음성을 얻을 수 있습니다.
오디오 프롬프트를 추가하거나 시드를 고정하여 화자 일관성을 유지할 수 있습니다 (가이드가 곧 제공될 예정입니다 - 지금은 Gradio의 두 번째 예제로 시도해보세요).

## 기능

- `[S1]`과 `[S2]` 태그를 통해 대화 생성
- `(웃음)`, `(기침)` 등 비언어적 표현 생성
  - 아래의 언어적 태그는 인식되지만 예상치 못한 출력이 나올 수 있습니다.
  - `(웃음), (목청을 가다듬음), (한숨), (놀람), (기침), (노래), (노래함), (중얼거림), (삐 소리), (신음), (코를 킁킁거림), (박수), (비명), (숨을 들이쉼), (숨을 내쉼), (박수갈채), (트림), (흥얼거림), (재채기), (낄낄거림), (휘파람)`
- 음성 복제. 자세한 내용은 [`example/voice_clone.py`](example/voice_clone.py)를 참조하세요.
  - Hugging Face 스페이스에서는 복제하고 싶은 오디오를 업로드하고 스크립트 앞에 해당 대본을 배치할 수 있습니다. 대본이 필요한 형식을 따르는지 확인하세요. 그러면 모델은 스크립트의 내용만 출력합니다.

## ⚙️ 사용법

### Python 라이브러리로 사용하기

```python
from dia.model import Dia


model = Dia.from_pretrained("nari-labs/Dia-1.6B", compute_dtype="float16")

text = "[S1] Dia는 오픈 가중치 텍스트-대화 모델입니다. [S2] 스크립트와 음성을 완전히 제어할 수 있습니다. [S1] 와. 놀랍네요. (웃음) [S2] GitHub나 Hugging Face에서 지금 바로 시도해보세요."

output = model.generate(text, use_torch_compile=True, verbose=True)

model.save_audio("simple.mp3", output)
```

pypi 패키지와 작동하는 CLI 도구는 곧 제공될 예정입니다.

## 💻 하드웨어 및 추론 속도

Dia는 GPU에서만 테스트되었습니다 (pytorch 2.0+, CUDA 12.6). CPU 지원은 곧 추가될 예정입니다.
초기 실행은 Descript Audio Codec도 다운로드해야 하므로 더 오래 걸립니다.

RTX 4090에서 벤치마크한 속도는 다음과 같습니다.

| 정밀도 | 컴파일 사용 시 실시간 계수 | 컴파일 미사용 시 실시간 계수 | VRAM |
|:-:|:-:|:-:|:-:|
| `bfloat16` | x2.1 | x1.5 | ~10GB |
| `float16` | x2.2 | x1.3 | ~10GB |
| `float32` | x1 | x0.9 | ~13GB |

향후 양자화 버전을 추가할 예정입니다.

하드웨어가 없거나 더 큰 버전의 모델을 사용하고 싶다면 [여기](https://tally.so/r/meokbo)에서 대기자 명단에 참여하세요.

## 🪪 라이선스

이 프로젝트는 Apache License 2.0으로 라이선스되어 있습니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

## ⚠️ 면책 조항

이 프로젝트는 연구 및 교육 목적으로 사용하기 위한 고품질 음성 생성 모델을 제공합니다. 다음 사용은 **엄격히 금지**됩니다:

- **신원 오용**: 허가 없이 실제 개인을 닮은 오디오를 생성하지 마세요.
- **기만적 콘텐츠**: 이 모델을 사용하여 오해를 불러일으키는 콘텐츠(예: 가짜 뉴스)를 생성하지 마세요.
- **불법 또는 악의적 사용**: 이 모델을 불법이거나 해를 끼치기 위한 활동에 사용하지 마세요.

이 모델을 사용함으로써 귀하는 관련 법적 기준과 윤리적 책임을 준수하는 데 동의합니다. 우리는 이 기술의 비윤리적 사용에 대해 **책임을 지지 않으며** 강력히 반대합니다.

## 🔭 TODO / 향후 작업

- ARM 아키텍처와 MacOS를 위한 Docker 지원.
- 추론 속도 최적화.
- 메모리 효율성을 위한 양자화 추가.

## 🤝 기여하기

우리는 1명의 전임 연구 엔지니어와 1명의 파트타임 연구 엔지니어로 구성된 작은 팀입니다. 모든 기여를 환영합니다!
토론을 위해 [디스코드 서버](https://discord.gg/yBrqQ9Dd)에 참여하세요.

## 🤗 감사의 말

- [Google TPU Research Cloud 프로그램](https://sites.research.google/trc/about/)에서 제공한 컴퓨팅 리소스에 감사드립니다.
- 우리의 작업은 [SoundStorm](https://arxiv.org/abs/2305.09636), [Parakeet](https://jordandarefsky.com/blog/2024/parakeet/), [Descript Audio Codec](https://github.com/descriptinc/descript-audio-codec)에서 큰 영감을 받았습니다.
- ZeroGPU Grant를 제공한 Hugging Face에 감사드립니다.
- "Nari"는 순수 한국어로 백합을 의미합니다.
- 데이터 필터링에 도움을 준 Jason Y.에게 감사드립니다.

## ⭐ 스타 히스토리

<a href="https://www.star-history.com/#nari-labs/dia&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=nari-labs/dia&type=Date" />
 </picture>
</a>
