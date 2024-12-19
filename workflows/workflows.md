# Workflows

* [SE_FLUX_txt2img_v21](#se_flux_txt2img_v21): GGUF-quantized Flux text to image with LoRA tag loading and optional
  background removal
* [SE_FLUX_img2img_v21](#se_flux_img2img_v21): GGUF-quantized Flux image to image with LoRA tag loading and optional
  background removal
* [SE_SD_txt2img_v21](#se_sd_txt2img_v21): SD text to image with LoRA tag loading and optional background removal
* [SE_SD_img2img_v21](#se_sd_img2img_v21): SD image to image with LoRA tag loading and optional background removal

## Installation

Load `SE_*_*_v21.json` into ComfyUI and use Manager to install any custom nodes you don't have.

Check that the workflow runs. You may have to adjust the paths to the models and CLIPs. This is fine, as long as it
runs.

Now load `SE_*_*_v21_ST.json` into SillyTavern. Open it in Workflow Editor (Extensions > Image Generation > ComfyUI >
ComfyUI Workflow > Edit).

Add a custom parameter `bgr_threshold`. Set it to `0` for now, to disable background removal. You can adjust this later.

Go back to Extensions > Image Generation and set the values of the model, VAE, etc to the values that worked in ComfyUI.
You should now be able to generate images from SillyTavern.

### Custom nodes

* [Load Image (Base64)](https://github.com/Acly/comfyui-tooling-nodes): loads images from base64 strings, used for
  image-to-image workflows

* [Load LoRA Tag](https://github.com/badjeff/comfyui_lora_tag_loader): loads LoRAs from tags in prompts. So much easier
  to load multiple LoRAs by passing them as prompt tags than trying to add UI for it in ST.

* [ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF): loads quantized models, because otherwise I don't have enough
  VRAM to run FLUX and LLM at the same time

* [ComfyUI-Inspyrenet-Rembg](https://github.com/john-mnz/ComfyUI-Inspyrenet-Rembg): background removal, works more often
  than other rembg alternatives in my testing

## Usage

**Standard generation**: `/sd a bear`

**Generation with background removal:** set `bgr_threshold` to `0.5` in workflow editor

**Generation with flux.D**: change your model to `GGUF: flux1 dev Q4 0` in Extensions > Image Generation > ComfyUI or
use `/sd model=flux1-dev-Q4_0.gguf `

**Switchable options at generation-time**:

- set `bgr_threshold` to `{{getvar::bgr_threshold}}` in workflow editor
- `/setvar key=bgr_threshold true | /sd a tree`

**Switchable options using generation styles**:

- Make a style "No background" with common prompt prefix `{{setvar::bgr_threshold::0}}`
- `/imagine-style No background | /sd a forest`

Yes, you can set variables in the style prefix and insert their values into the ComfyUI workflow. Isn't that wild?

**Generate an image and use it as a temporary chat sprite**:

See [Sprite Autogen](../autogen/autogen.md) for more details.

**Add character's current expression to the generation**:

Modify the "yourself" prompt to insert the character's current expression, using the variable that is set by the
previous QR:

```
{{char}}'s current facial expression is: {{getvar::lastsprite}}. ... must include physical features, facial expression, and appearance
```

## Configuration

### SE_FLUX_txt2img_v21

GGUF-quantized Flux text to image with LoRA tag loading and optional background removal

#### Files

* `SE_FLUX_txt2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_FLUX_txt2img_v21_ST.json`: Placeholder version for use in SillyTavern

#### Notes

- **Model**: GGUF Unet such as `flux1-schnell-Q4_K_S.gguf` or `flux1-dev-Q4_0.gguf`, from the SillyTavern models
  dropdown as of `staging` PR #3088
- **VAE**: Flux VAE such as `ae.safetensors`
- Flux CLIPs `clip_l.safetensors` and `t5/google_t5-v1_1-xxl_encoderonly-fp8_e4m3fn.safetensors` must be in `clips`
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

#### Replacements:

* `%width%`: Empty Latent Image `width`
* `%height%`: Empty Latent Image `height`
* `%seed%`: KSampler `seed`
* `%steps%`: KSampler `steps`
* `%scale%`: KSampler `cfg`
* `%sampler%`: KSampler `sampler_name`
* `%scheduler%`: KSampler `scheduler`
* `%denoise%`: KSampler `denoise`
* `%prompt%`: Load LoRA Tag `text`
* `%vae%`: Load VAE `vae_name`
* `%bgr_threshold%`: background removal threshold `threshold`
* `%model%`: Unet Loader (GGUF) `unet_name`

#### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* DualCLIPLoader `clip_name1`: `"clip_l.safetensors"`
* DualCLIPLoader `clip_name2`: `"t5/google_t5-v1_1-x`
* DualCLIPLoader `type`: `"flux"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Empty Latent Image `batch_size`: `1`

### SE_FLUX_img2img_v21

GGUF-quantized Flux image to image with LoRA tag loading and optional background removal

#### Files

* `SE_FLUX_img2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_FLUX_img2img_v21_ST.json`: Placeholder version for use in SillyTavern

#### Notes

- **Model**: GGUF Unet such as `flux1-schnell-Q4_K_S.gguf` or `flux1-dev-Q4_0.gguf`, from the SillyTavern models
  dropdown as of `staging` PR #3088
- **VAE**: Flux VAE such as `ae.safetensors`
- Flux CLIPs `clip_l.safetensors` and `t5/google_t5-v1_1-xxl_encoderonly-fp8_e4m3fn.safetensors` must be in `clips`
- **Source image**: the current character avatar is the source image
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

#### Replacements:

* `%seed%`: KSampler `seed`
* `%steps%`: KSampler `steps`
* `%scale%`: KSampler `cfg`
* `%sampler%`: KSampler `sampler_name`
* `%scheduler%`: KSampler `scheduler`
* `%denoise%`: KSampler `denoise`
* `%char_avatar%`: Load Image (Base64) `image`
* `%prompt%`: Load LoRA Tag `text`
* `%vae%`: Load VAE `vae_name`
* `%bgr_threshold%`: background removal threshold `threshold`
* `%width%`: Resize image `resize_width`
* `%height%`: Resize image `resize_height`
* `%model%`: Unet Loader (GGUF) `unet_name`

#### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* DualCLIPLoader `clip_name1`: `"clip_l.safetensors"`
* DualCLIPLoader `clip_name2`: `"t5/google_t5-v1_1-x`
* DualCLIPLoader `type`: `"flux"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Resize image `mode`: `"resize"`
* Resize image `supersample`: `"true"`
* Resize image `resampling`: `"lanczos"`
* Resize image `rescale_factor`: `2`

### SE_SD_txt2img_v21

SD text to image with LoRA tag loading and optional background removal

#### Files

* `SE_SD_txt2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_SD_txt2img_v21_ST.json`: Placeholder version for use in SillyTavern

#### Notes

- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

#### Replacements:

* `%negative_prompt%`: CLIP Text Encode (Negative Prompt) `text`
* `%width%`: Empty Latent Image `width`
* `%height%`: Empty Latent Image `height`
* `%seed%`: KSampler `seed`
* `%steps%`: KSampler `steps`
* `%scale%`: KSampler `cfg`
* `%sampler%`: KSampler `sampler_name`
* `%scheduler%`: KSampler `scheduler`
* `%denoise%`: KSampler `denoise`
* `%model%`: Load Checkpoint `ckpt_name`
* `%prompt%`: Load LoRA Tag `text`
* `%bgr_threshold%`: background removal threshold `threshold`

#### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Empty Latent Image `batch_size`: `1`

### SE_SD_img2img_v21

SD image to image with LoRA tag loading and optional background removal

#### Files

* `SE_SD_img2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_SD_img2img_v21_ST.json`: Placeholder version for use in SillyTavern

#### Notes

- **Source image**: the current character avatar is the source image
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

#### Replacements:

* `%negative_prompt%`: CLIP Text Encode (Negative Prompt) `text`
* `%seed%`: KSampler `seed`
* `%steps%`: KSampler `steps`
* `%scale%`: KSampler `cfg`
* `%sampler%`: KSampler `sampler_name`
* `%scheduler%`: KSampler `scheduler`
* `%denoise%`: KSampler `denoise`
* `%model%`: Load Checkpoint `ckpt_name`
* `%char_avatar%`: Load Image (Base64) `image`
* `%prompt%`: Load LoRA Tag `text`
* `%bgr_threshold%`: background removal threshold `threshold`
* `%width%`: Resize image `resize_width`
* `%height%`: Resize image `resize_height`

#### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Resize image `mode`: `"resize"`
* Resize image `supersample`: `"true"`
* Resize image `resampling`: `"lanczos"`
* Resize image `rescale_factor`: `2`
