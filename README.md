# FLUX 3 Image Klein — ComfyUI Workflows (int8)

Two ready-to-run ComfyUI workflows for Black Forest Labs' **FLUX 3 Image Klein** (distilled, int8), built on BFL's official workflows. No custom nodes. Tested on a 16 GB GPU.

| Workflow | What it does |
|---|---|
| [`FLUX3_Klein_int8_Text_to_Image.json`](workflows/FLUX3_Klein_int8_Text_to_Image.json) | Prompt → image. Optional prompt upsampler and optional bounding boxes. |
| [`FLUX3_Klein_int8_Image_Edit.json`](workflows/FLUX3_Klein_int8_Image_Edit.json) | Edit a photo with a written instruction. Up to 4 reference images, optional bounding boxes, and a before/after slider. |

---

## Requirements

- **ComfyUI 0.41 or newer** (tested on 0.42.1). Older versions don't have the FLUX 3 nodes.
- **Nodes 2.0** turned on (ComfyUI icon → Nodes 2.0) if you want to draw bounding boxes.
- No custom nodes.

## Models

The model repo is **gated**. Sign in to Hugging Face and accept the license on **[black-forest-labs/flux-3-image-klein-comfy](https://huggingface.co/black-forest-labs/flux-3-image-klein-comfy)** first. It must be that exact page, since accepting on another FLUX page doesn't count. If you download with a fine-grained token, enable *"Read access to contents of all public gated repos you can access"*.

| File | Size | Folder |
|---|---|---|
| `flux3-image-klein-int8-convrot.safetensors` | 5.83 GB | `ComfyUI/models/diffusion_models/` |
| `qwen3vl_4b_int8_convrot.safetensors` | 4.86 GB | `ComfyUI/models/text_encoders/` |
| `qwen3.5_9b_flux3_prompt_upsampler_w4a8.safetensors` | 6.72 GB | `ComfyUI/models/text_encoders/` |
| `flux2-vae.safetensors` | 0.34 GB | `ComfyUI/models/vae/` |

> **The upsampler file is required even if you keep the upsampler off.** ComfyUI checks every model file before it runs.

> **Old file names:** some templates still ask for `flux3_image_klein_sd_int8_convrot.safetensors`. That file was renamed. The distilled int8 model is now `flux3-image-klein-int8-convrot.safetensors`. Files with `base` in the name are the 50-step base model, not the one these workflows use.

## Sample images (Image Edit)

The edit workflow loads BFL's sample images by default. Download `klein_ref_*.jpg` from the [`workflows/` folder of the model repo](https://huggingface.co/black-forest-labs/flux-3-image-klein-comfy/tree/main/workflows) into `ComfyUI/input/`, or load your own photo.

---

## How to use

### Text to Image
1. Type your prompt in **Prompt**.
2. **Prompt Upsampler → enable upsampler**: on rewrites a short prompt into a detailed one. Off sends your prompt exactly as written. The **Upsampled prompt** box shows what the model received.
3. **Resolution**: pick the aspect ratio and megapixels. The default is 4 MP; 1–3 MP is faster.
4. **Diffusion (FLUX 3 Klein)**: set the batch size and seed. Leave steps (16), alpha (16), sampler (euler) and scheduler (simple) at BFL's defaults.

### Image Edit
1. Load your photo into **Image 1**.
2. Write the edit in **Prompt**, for example *"Change the car's paint to glossy orange. Keep everything else unchanged."*
3. To use more references, select **Image 2** (or 3 and 4) and press **Ctrl+M** to unmute it. In the prompt, call them *"the first image"*, *"the second image"*.
4. Optional: draw a box in **Bounding boxes**, give it a description, and cite it in the prompt as `<obj_1>` (with `<ref_image_0>` for the photo).
5. Drag the **Before / after** slider to check what changed.

Outputs save to `ComfyUI/output/flux3_klein/`.

---

## Prompting tips

- **Write long, specific captions** with the upsampler off: the type of photo, the lighting, the camera, frame positions (*frame-left*, *upper-right*), colors and materials. Short prompts work better with the upsampler on.
- **Put any text you want in the image inside double quotes.** Spelling is very reliable.
- **Edits:** state the change, then end with one sentence listing what must stay unchanged.
- **Realistic people:** describe physical texture rather than quality words, for example *"visible pores, matte skin, dry frizzy hair"*. Without that, skin and hair can look glossy.

## Known limits

- Small text that is part of an edited photo, like badges or plates, can get scrambled.
- If the edit needs content that isn't in your photo (for example the area below the frame), the model invents it.
- Results vary between seeds. Run a batch and pick the best.

---

## Changes from BFL's original workflows

- **Image Edit** is based on `flux3_klein_int8_mi2i.json`. Image 2 is muted by default, so a single-image edit works out of the box, and a **Before / after** compare node is added.
- Output filename prefixes are set to `flux3_klein/t2i` and `flux3_klein/edit`.
- All sampling settings are unchanged from BFL's defaults.

## License

The workflow files are MIT licensed. See [LICENSE](LICENSE); the original workflows are © Comfy Org. The **model weights** are covered by Black Forest Labs' own license on the Hugging Face page, so read it before any commercial use.

File checksums are in [`SHA256SUMS`](SHA256SUMS).
