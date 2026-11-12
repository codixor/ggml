# ggml-shared

ggml as vendored by audio.cpp (0xShug0/audio.cpp `external/ggml`, 0.12 fork,
pinned at audio.cpp 9d409dae877425d8d179d24fcdd8848fc9476c78) plus the
vision.cpp additions from `depend/llama`:

- `ggml_concat_n`, `GGML_OP_CONV_2D_DEFORM`, channel-last (cwhn) conv on CPU
  (commit 6ef8aa4a1), with the GEMM barrier fix (d62fcf3fb)
- Vulkan conv2d_deform, repeat f16, `ggml_backend_dev_supports_f16`
  (commit 5fde0fae4); the channel-last Vulkan im2col was left out because
  every shipped model uses WHCN on GPU

Build vision.cpp with `-DVISP_SHARED_GGML=ON`, whisper.cpp as a
subdirectory of the same project (it reuses the parent `ggml` target), and
audio.cpp with `-DAUDIOCPP_GGML_SOURCE_DIR=<this folder>`. One ggml then
serves images, speech-to-text and text-to-speech.

Verified 2026-11-12: all 11 vision models match the default build; Whisper
large-v3-turbo transcript identical; Audio8 TTS byte-identical with a fixed
seed; Mac CPU/Metal and Windows CPU/Vulkan (RX 580).
