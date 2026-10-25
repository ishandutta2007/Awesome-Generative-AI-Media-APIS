# Awesome-Generative-AI-Media-APIS

# Top Generative AI Media API Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on Self-Hosted Inference Gateways, OpenAI-Compatible Endpoints & Multi-Model Orchestration*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Generative AI Media APIs**. These tools expose image, video, audio, and speech generation capabilities through standardized HTTP interfaces — with a spectrum running from commercial API gateways like Fal.ai and Replicate to open-source servers that turn your own GPU into a private, OpenAI-compatible media generation endpoint.

**Examples** include ComfyUI, LocalAI, inferhost, InvokeAI, and genblaze-core (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, OpenAI-compatible endpoints, and transparent model routing — especially relevant as **ComfyUI's API** lets you deploy workflows as production endpoints , and **inferhost** wraps llama.cpp and stable-diffusion.cpp behind a single OpenAI-compatible gateway for chat, speech, and images on one port .

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **Fal.ai** — **The serverless inference standard for generative media.** Python and JS SDKs for serverless image/video generation, plus a Vercel AI SDK provider . **Pay-per-generation pricing**. **Best for developers** wanting fast cold starts and a broad model catalog without managing GPU infrastructure, accepting per-request costs and cloud dependency .

- **Replicate** — **The serverless model hosting platform.** Hosts open-source image models with a simple API and Cog packaging format. **Pay-per-second GPU billing**. **Best for teams** wanting to run community models without local hardware, accepting that models are hosted on Replicate's infrastructure .

- **Stability AI API** — **The commercial gateway to Stable Diffusion models.** SD 3.5 and Stable Image Ultra via API with a free credit tier. **Best for teams** wanting Stable Diffusion quality without local GPU setup, accepting per-image costs and cloud dependency.

- **OpenAI Images API** — **The reference text-to-image API.** GPT Image and DALL·E 3 via `client.images.generate()` and `client.images.edit()` . **Pay-per-image API**. **Best for developers** wanting strong semantic understanding and simple integration, accepting content policies and cloud dependency .

- **Black Forest Labs API** — **The commercial Flux API.** Direct access to Flux models via API. **Best for teams** wanting Flux quality with commercial support, accepting cloud dependency and licensing terms.

## Open-Source GitHub Projects

- **ComfyUI** — **The node-graph standard with a production API.** **Open-source, Python-based**, with a **built-in HTTP server on port 8188** that accepts workflow submissions, uploads, and output retrieval via REST and WebSocket APIs . **Comfy API** (paid Comfy plan) packages any workflow into an autoscaling production endpoint with immutable releases — the same environment you test is the one you deploy . **The honest caveat**: **the API expects workflow JSON**, not simple prompt strings — you are submitting a graph, not a single parameter, and building the workflow is the real work . **Best for teams** wanting total control over the generation pipeline and reproducible, versioned workflows exposed as APIs .

- **LocalAI** — **The OpenAI-compatible multi-modal inference server.** **Open-source**, exposing `/v1/images/generations`, `/v1/audio/speech`, and `/v1/chat/completions` on a single endpoint . **Image generation backends**: `stablediffusion-ggml` and `diffusers`, with **distributed RPC workers** for sharding models across machines . **Includes `/v1/images/upscale`** via the Stable Diffusion x4 upscaler gallery model . **The honest caveat**: **model configuration files** are required per model — you define backends, parameters, and schedulers in YAML before the endpoint works . **Best for teams** wanting a single OpenAI-compatible gateway covering text, image, and speech on their own hardware .

- **inferhost** — **The private GPU gateway for every modality.** **Open-source**, wrapping **llama.cpp and stable-diffusion.cpp** behind a **single OpenAI-compatible endpoint on port 9001** — chat, vision, speech, and images from one URL . **`/v1/images/generations`** returns base64 PNG; **`/v1/audio/speech`** returns WAV . **Paste a Hugging Face repo and it auto-detects** single-file models (SD 1.5/SDXL) and multi-file models (Flux, Z-Image, Qwen-Image) with companion VAE and text encoders . **One GPU, many models**: llama-swap lazy-loads and hot-swaps models in/out of VRAM, so a 24 GB card serves a 27B LLM and Flux image generation . **Image editing** via `/v1/images/edits` is reachable directly on llama-swap (port 9090) because the gateway doesn't route multipart by model . **The honest caveat**: **image editing skips the main gateway** — you call llama-swap's `/upstream/<model>/v1/images/edits` path directly . **Best for teams** wanting a private, multi-modal OpenAI-compatible gateway with model hot-swapping on a single GPU .

- **InvokeAI** — **The professional studio UI with an API.** **Open-source (Apache 2.0)**, with OpenAI models configurable via `api_keys.yaml` — GPT Image family and DALL·E 3 are routed transparently: pure text-to-image hits `/v1/images/generations`, while any request with an init image goes to `/v1/images/edits` . **Batch limits**: GPT Image tops out at 10 per request; DALL·E 3 is limited to 1 . **Inpainting (mask-based)** is **not supported** for any OpenAI model in Invoke. **The honest caveat**: **OpenAI integration is for image generation only** — Invoke's own models run locally, but the OpenAI models are cloud API calls, mixing local and cloud in one UI . **Best for professionals** wanting a polished studio with optional cloud model access alongside local generation.

- **genblaze-core** — **The provider-agnostic pipeline orchestrator with provenance.** **MIT licensed**, from Backblaze, giving a **single Pipeline API** for text-to-video, text-to-image, text-to-speech, image-to-video, and audio generation — swap Sora, Veo, Runway, Luma, Flux, DALL·E, ElevenLabs, Stable Audio, LMNT, and GMICloud without rewriting pipeline logic . **Built-in SHA-256 provenance**: every run emits a tamper-evident manifest capturing provider, model, prompt, parameters, timestamps, and output metadata, embeddable into PNG/JPEG/MP4/MP3/WAV files . **Storage-agnostic sinks** for S3, B2, R2, MinIO, Parquet, or local disk . **The honest caveat**: **provider adapters are separate packages** (`genblaze-openai`, `genblaze-runway`, etc.) — it is an orchestration layer, not a model host . **Best for teams** building multi-provider media pipelines needing reproducibility and tamper-evidence, not a direct API server .

- **Aquiles-Image** — **The diffusers-based OpenAI-compatible image server.** **Open-source**, with `client.images.generate()` working against a local server at `http://127.0.0.1:5500` . **GGUF support** via a curated registry: `aquiles-image gguf-download --model-id flux1-dev-q4k` then `aquiles-image serve --model "gguf:flux1-dev-q4k"` — only the transformer is quantized, with text encoder and VAE from the base repo . **CPU offloading** for SD3/SD3.5 on low-VRAM GPUs (`--cpu-offload`), fixing OOM on 16 GB cards . **LoRA support** via a JSON config . **The honest caveat**: **GGUF and `torch.compile` are incompatible** — CPU offload is applied automatically for GGUF . **Best for teams** wanting an OpenAI-compatible image endpoint with quantized model support on modest VRAM .

- **speech-server** — **The TTS API server.** **MIT licensed**, FastAPI-based, with **Chatterbox TTS and Kokoro ONNX** backends . **`/tts` endpoint** returns WAV; **`/voices`** lists available voices . **Voice cloning support, streaming endpoint, and YAML config** . **The honest caveat**: **TTS only** — it is a speech generation server, not an image or video API, and the two backends have different dependency footprints . **Best for teams** needing a self-hosted TTS endpoint without a cloud speech API .

### Additional Strong Open-Source Options

- **AUTOMATIC1111 / Forge** — **The de facto Stable Diffusion WebUI API.** Exposes `/sdapi/v1/txt2img` on port 7860 with its own JSON schema (base64 images out) . **Widely supported by clients** (e.g. Go's `imagegen` package) . **The honest caveat**: **its own schema, not OpenAI-compatible** — clients must speak `sdapi`, not `images.generate()` . **Best for users** already running A1111 and wanting its extension ecosystem exposed programmatically .
- **museq** — **The lazy-load OpenAI-compatible server.** `muse serve` exposes `/v1/audio/speech`, `/v1/embeddings`, `/v1/images/embeddings`, and `/v1/chat/completions` on port 8000 . **Models stay on disk until the first request that names them**, then a worker spawns on demand . **Bundled model pulling** via `muse pull soprano-80m` . **Best for teams** wanting a single server covering TTS, embeddings, and chat with lazy model loading .

**Frameworks for building custom systems**: Combine **inferhost** or **LocalAI** for a unified OpenAI-compatible gateway across image, speech, and chat, **ComfyUI** for workflow-as-API with reproducible node graphs, **Aquiles-Image** for a diffusers-based image endpoint with GGUF quantization, and **genblaze-core** for multi-provider pipeline orchestration with provenance — to build a generative media API stack tailored to your need for a single gateway versus workflow control, and local versus cloud model execution.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Generative media API tools differ significantly in API surface (OpenAI-compatible vs custom schema vs workflow JSON), model hosting (local vs cloud adapters), and licensing. **ComfyUI's API expects a workflow graph, not a prompt string** — it is powerful but not a drop-in OpenAI replacement . **LocalAI and inferhost expose OpenAI-compatible endpoints** for images and speech, but require per-model configuration files (LocalAI) or Hugging Face repo setup (inferhost) before the endpoints work . **Aquiles-Image's GGUF path is incompatible with `torch.compile`** . **genblaze-core is an orchestration layer with separate provider adapters**, not a model server . **A1111/Forge uses its own `sdapi` schema**, not the OpenAI API . **Cloud APIs (Fal, Replicate, Stability, OpenAI) are faster to try but slower to own** — vendor terms and per-generation costs apply .
- Self-hosted open-source solutions require proper GPU drivers, sufficient VRAM for the chosen models, and awareness that multi-modal gateways (inferhost, LocalAI) may evict one model to load another when VRAM is constrained.

---

**Made for developers and teams choosing generative media API infrastructure that matches their need for OpenAI compatibility, workflow control, and data sovereignty.**
Let's make generative AI media APIs more open, OpenAI-compatible, and self-hostable.
