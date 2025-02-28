# AI no jimaku gumi (AIの字幕組)

AI no jimaku gumi is a cli utility to facilitate the translation of video.

## Installation

To get started with AI no jimaku gumi, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/Inokinoki/ai-no-jimaku-gumi.git
    ```
2. Navigate to the project directory:
    ```bash
    cd ai-no-jimaku-gumi
    ```
3. Build with cargo:
    ```bash
    cargo build
    ```
4. Download whisper model(you can also download other models refer: https://huggingface.co/ggerganov/whisper.cpp):
    ```bash
    wget https://huggingface.co/ggerganov/whisper.cpp/resolve/main/ggml-tiny.bin
    ```
5. Run it with your video path after `--input-video-path` and target language after `-t`.

## Usage

To use AI no jimaku gumi, refer this help:

```
aI NO jimaKu gumI, a sub-title maker using AI.

Usage: ainojimakugumi [OPTIONS] --input-video-path <INPUT_VIDEO_PATH>

Options:
  -i, --input-video-path <INPUT_VIDEO_PATH>
          Name of the person to greet
      --source-language <SOURCE_LANGUAGE>
          Which language to translate from (default: "ja") (possible values: "en", "es", "fr", "de", "it", "ja", "ko", "pt", "ru", "zh") (example: "ja") [default: ja]
      --target-language <TARGET_LANGUAGE>
          Which language to translate to (default: "en") (possible values: "en", "es", "fr", "de", "it", "ja", "ko", "pt", "ru", "zh") (example: "en") [default: en]
      --start-time <START_TIME>
          Video start time [default: 0]
      --end-time <END_TIME>
          Video end time [default: 0]
      --subtitle-source <SUBTITLE_SOURCE>
          Subtitle source (default: "audio") (possible values: "audio", "container", "ocr") (example: "audio") (long_about: "Subtitle source to use") [default: audio]
      --ggml-model-path <GGML_MODEL_PATH>
          ggml model path (default: "ggml-tiny.bin") (example: "ggml-tiny.bin", ggml-small.bin") (long_about: "Path to the ggml model") [default: ggml-tiny.bin]
      --only-extract-audio
          Only extract the audio (default: false) (long_about: "Only extract the audio, if subtitle source is audio, but do not transcribe (Debug purpose)") (example: true)
      --only-transcript
          Only save the transcripted subtitle (default: false) (long_about: "Only save the transcripted subtitle but do not translate (Debug purpose)") (example: true)
      --original-subtitle-path <ORIGINAL_SUBTITLE_PATH>
          Original subtitle SRT file path (default: "") (example: "origin.srt") (long_about: "Original subtitle path to save the transcripted subtitle as SRT") [default: ]
      --only-translate
          Only translate the subtitle (default: false) (long_about: "Only translate the subtitle but do not export (Debug purpose)")
  -s, --subtitle-backend <SUBTITLE_BACKEND>
          Subtitle backend (default: "srt") (possible values: "srt", "container", "embedded") (example: "srt") (long_about: "Subtitle backend to use") [default: srt]
      --subtitle-output-path <SUBTITLE_OUTPUT_PATH>
          Subtitle output path (if srt) (default: "output.srt") (example: "output.srt") (long_about: "Subtitle output path (if srt)") [default: output.srt]
  -t, --translator-backend <TRANSLATOR_BACKEND>
          Translator backend (default: "deepl") (possible values: "deepl", "google", "llm") (example: "google") (long_about: "Translator backend to use") [default: deepl]
      --llm-model-name <LLM_MODEL_NAME>
          Model name (if llm) (default: "gpt-4o") (example: "gpt-4o") (long_about: "Model name (if using llm for translation)") [default: gpt-4o]
      --llm-api-base <LLM_API_BASE>
          API base (if llm) (default: "https://api.openai.com") (example: "https://api.openai.com") (long_about: "API base used in `genai` crate (if using llm for translation)") [default: https://api.openai.com]
      --llm-prompt <LLM_PROMPT>
          Prompt (if llm) (default: "") (example: "Translate the following text to English") (long_about: "Prompt (if using llm for translation)") [default: ]
  -h, --help
          Print help
  -V, --version
          Print version
```

We are currently supporting only `deepl, llm` translation and `srt` export.

### Translator backend

You might need to follow the specific instructions to use a translator backend:

- `deepl` (default): please provide your own DeepL API key in `DEEPL_API_KEY` env, and `DEEPL_API_URL=https://api.deepl.com` if you are using the paid API version.
- `llm`: if you are using llm translate, please refer the repo [rust-genai](https://github.com/jeremychone/rust-genai) for more detail. An example here:
```cli
export CUSTOM_API_KEY=sk-xxxxxxxxxxxxxxxxxxxxxxx
./target/debug/ainojimakugumi --input-video-path one.webm \
    --translator-backend llm \
    --llm-api-base https://sssss.com/v1/ \
    --llm-prompt 'translate this to English' \
    --llm-model-name 'gpt-4o-mini'
    --ggml-model-path ggml-small.bin
```
