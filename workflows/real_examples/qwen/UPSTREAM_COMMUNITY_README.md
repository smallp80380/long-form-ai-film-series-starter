# Qwen-Image 2.1 ComfyUI Workflow

A ready-to-run ComfyUI workflow for Qwen-Image 2.1, using the GGUF Q8_0 quantization. It does text-to-image by default, with an optional depth/reference image branch and a three-slot LoRA stack you can switch on without rewiring anything.

**English** · [简体中文](README.zh-CN.md) · [日本語](README.ja.md)

## Preview

<p align="center">
  <img src="assets/01-low-key-9x16.webp" width="32%" alt="Low-key interior, 9:16">
  <img src="assets/02-soft-daylight-9x16.webp" width="32%" alt="Soft daylight interior, 9:16">
  <img src="assets/03-warm-interior-9x16.webp" width="32%" alt="Warm interior, 9:16">
</p>

All three were generated at `9:16 (Portrait Widescreen)`, 1.0 MP (768×1376), 25 steps, CFG 1.0, euler / simple, no LoRA, reference switch off. Only the prompt differs. The files above are downscaled to 714×1280 to keep the repository small.

These are re-encoded to WebP and stripped of metadata, so dragging one onto the ComfyUI canvas will *not* restore a graph. Use the JSON in [`workflows/`](workflows/) for that.

## What's included

| Workflow | Purpose |
| --- | --- |
| [`workflows/qwen_image_2_1_Q8_0_t2i.json`](workflows/qwen_image_2_1_Q8_0_t2i.json) | Text-to-image with optional depth reference and LoRA stack |

## Requirements

### ComfyUI

**Minimum version: ComfyUI 0.37.0** (frontend 1.53.6), the version this workflow was built and tested on.

`EmptyLatentImage` emits a 4-channel latent at 1/8 scale, while Qwen-Image 2.1 wants 64 channels at 1/16. The graph works because recent ComfyUI carries a `downscale_ratio_spacial` hint into the sampler and rescales the empty latent for you. On builds without that hint, this workflow renders at double the size you asked for.

Four of the nodes used here are recent enough that an older ComfyUI will not have them. All four ship with ComfyUI itself rather than a custom pack:

- `TextEncodeQwenImage21` (`comfy_extras.nodes_qwen`): Qwen-Image 2.1 text encoding
- `ResolutionSelector` (`comfy_extras.nodes_resolution`): aspect-ratio-driven resolution picker
- `If/Else Switch` / `ComfySwitchNode` (`comfy_extras.nodes_logic`): the reference-image and latent branches
- `Boolean` / `PrimitiveBoolean` (`comfy_extras.nodes_primitive`): the one toggle driving both branches

If any of them shows up as a red "missing node" box, update ComfyUI before looking for a custom pack to install. `If/Else Switch` is flagged experimental, so it may be hidden from the node search menu until you enable experimental nodes. That only affects searching; loading this workflow works either way.

### Custom nodes

Install these two through ComfyUI Manager, or clone them into `ComfyUI/custom_nodes/`:

| Pack | Node used here | Repository |
| --- | --- | --- |
| ComfyUI-GGUF | `UnetLoaderGGUF` | https://github.com/city96/ComfyUI-GGUF |
| rgthree-comfy | `Power Lora Loader (rgthree)` | https://github.com/rgthree/rgthree-comfy |

Every other node in the graph is core ComfyUI.

### Models

Three files. Put each one in the folder listed below, under your ComfyUI install.

| File | Destination | Notes |
| --- | --- | --- |
| `qwen-image-2.1-Q8_0.gguf` | `ComfyUI/models/diffusion_models/` | The diffusion model. Older ComfyUI builds call this folder `models/unet/`, and the GGUF loader checks both, so either works. |
| `qwen3vl_8b_int8_convrot.safetensors` | `ComfyUI/models/text_encoders/` | Qwen3-VL 8B text encoder, int8. Loaded with type `qwen_image`. |
| `qwen_image_2.1_vae_bf16.safetensors` | `ComfyUI/models/vae/` | VAE, bf16. |

The filenames above are what the workflow expects. Search Hugging Face for each one to find a mirror: https://huggingface.co/models?search=qwen-image-2.1

If you pick a different quantization, the filename in the `UnetLoaderGGUF` node has to be changed to match.

## Usage

1. Install the two custom node packs and restart ComfyUI.
2. Place the three model files as described above.
3. Drag `workflows/qwen_image_2_1_Q8_0_t2i.json` onto the ComfyUI canvas.
4. Confirm the three loader nodes at the top-left picked up your filenames. If a dropdown is blank, the file isn't where ComfyUI is looking.
5. Pick an aspect ratio, write your prompt, and queue it.

## How the graph is wired

The workflow is split into six labelled groups:

- **Model**: `UnetLoaderGGUF` loads the GGUF and `CLIPLoader` loads the text encoder. Both run through the LoRA loader before reaching the sampler and the text encoder. The VAE loader sits alongside them.
- **Aspect ratio and latent size**: `ResolutionSelector` turns an aspect ratio plus a megapixel target into width/height, which drive `EmptyLatentImage`.
- **Prompt**: `TextEncodeQwenImage21` takes the prompt, the negative prompt, and the VAE. It emits positive and negative conditioning straight into the sampler.
- **Sampling and output**: an `If/Else Switch` picks which latent the sampler starts from, `EmptyLatentImage` or the one `TextEncodeQwenImage21` emits, then `KSampler` → `VAEDecode` → `SaveImage`. Images land in `ComfyUI/output/qwen-image-2.1/`.
- **LoRA**: the rgthree Power Lora Loader, three slots, all off. Both MODEL and CLIP pass through it.
- **Optional depth reference**: one `Boolean` toggle feeding two `If/Else Switch` nodes.

Two `Note` nodes on the canvas repeat the two things worth reading before you change anything: why CFG stays at 1.0, and what the reference switch actually does.

## Default parameters

| Setting | Value | Why |
| --- | --- | --- |
| Aspect ratio | `16:9 (Widescreen)` | Any of the eight presets work; this is just the shipped default. |
| Megapixels | `1.0` | About 1024×1024 worth of pixels, redistributed to the chosen ratio. |
| Multiple | `32` | Rounds width and height to a multiple of 32, which Qwen-Image prefers. |
| Reference resolution | `1024` | Only matters when the reference switch is on. Reference images are resized to about 1024×1024 worth of pixels, at multiples of 32, keeping their aspect ratio. `0` keeps each reference at its own size. |
| Steps | `25` | |
| CFG | `1.0` | **Leave this at 1.0.** Qwen-Image 2.1 is distilled; raising CFG burns the image. |
| Sampler | `euler` | |
| Scheduler | `simple` | |
| Denoise | `1.0` | Full denoise, since we start from an empty latent. |
| Seed | randomize | Fix the seed when you want to compare prompt edits. |

## LoRA

The Power Lora Loader has three slots, all disabled by default, pre-set to strengths 0.8, 0.6 and 0.4. Toggle a slot on and choose a file to use it.

Both the model and the CLIP path run through the loader, so a LoRA that carries text-encoder weights takes effect on the text encoder too.

Only Qwen-Image 2.1 LoRAs work here. LoRAs trained for Qwen-Image 1.x, SDXL or Flux will either fail to load or produce noise.

## Optional depth reference

The reference image switch is off, which makes this a pure text-to-image workflow. The `LoadImage` node is wired through an `If/Else Switch`, so when the switch is off nothing is passed to the text encoder at all. The path is cut, rather than weighted down to zero.

With the switch on, the loaded image is handed to `TextEncodeQwenImage21` as a reference, and the sampler switches from the `EmptyLatentImage` size over to the latent that `TextEncodeQwenImage21` emits. That second change is why the aspect ratio picker is deliberately bypassed while the switch is on: the node sizes its reference latents from Reference resolution, and sampling at any other size shifts the result. One `Boolean` node drives both switches, so there is still only one thing to toggle.

The shipped prompt opens with an instruction telling the model to read that image as a depth map: follow its composition, silhouettes, perspective and spatial layering, and ignore its greyscale colours. That sentence only matters when the switch is on; delete it for pure text-to-image if you prefer a clean prompt.

## Troubleshooting

**A node is red, or shows "missing node type".** Install the matching pack from the table above, or update ComfyUI if it's one of the four core nodes.

**A loader dropdown is empty.** ComfyUI only lists files it found at startup. Check the filename and folder, then restart ComfyUI.

**Out of memory.** Q8_0 is the heaviest common quant. Swap the `UnetLoaderGGUF` filename for a smaller one (Q6_K, Q5_K_M, Q4_K_M). Quality drops gradually and VRAM use drops a lot.

**Washed-out or fried images.** Check CFG. It should be 1.0.

**The reference image is being ignored.** That's the default. Turn the boolean switch on.

## Contributing

Bug reports and new workflow variants are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE). The workflow files are covered by this license; the models it loads are not, and carry their own terms.
