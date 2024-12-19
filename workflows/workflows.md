# Workflows

* [SE_FLUX_txt2img_v21](#se_flux_txt2img_v21): GGUF-quantized Flux text to image with LoRA tag loading and optional background removal
* [SE_FLUX_img2img_v21](#se_flux_img2img_v21): GGUF-quantized Flux image to image with LoRA tag loading and optional background removal
* [SE_SD_txt2img_v21](#se_sd_txt2img_v21): SD text to image with LoRA tag loading and optional background removal
* [SE_SD_img2img_v21](#se_sd_img2img_v21): SD image to image with LoRA tag loading and optional background removal

## SE_FLUX_txt2img_v21

GGUF-quantized Flux text to image with LoRA tag loading and optional background removal


### Files

* `SE_FLUX_txt2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_FLUX_txt2img_v21_ST.json`: Placeholder version for use in SillyTavern

### Notes

- **Model**: GGUF Unet such as `flux1-schnell-Q4_K_S.gguf` or `flux1-dev-Q4_0.gguf`, from the SillyTavern models dropdown as of `staging` PR #3088
- **VAE**: Flux VAE such as `ae.safetensors`
- Flux CLIPs `clip_l.safetensors` and `t5/google_t5-v1_1-xxl_encoderonly-fp8_e4m3fn.safetensors` must be in `clips`
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

### Replacements:

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

### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* DualCLIPLoader `clip_name1`: `"clip_l.safetensors"`
* DualCLIPLoader `clip_name2`: `"t5/google_t5-v1_1-x`
* DualCLIPLoader `type`: `"flux"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Empty Latent Image `batch_size`: `1`

## SE_FLUX_img2img_v21

GGUF-quantized Flux image to image with LoRA tag loading and optional background removal


### Files

* `SE_FLUX_img2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_FLUX_img2img_v21_ST.json`: Placeholder version for use in SillyTavern

### Notes

- **Model**: GGUF Unet such as `flux1-schnell-Q4_K_S.gguf` or `flux1-dev-Q4_0.gguf`, from the SillyTavern models dropdown as of `staging` PR #3088
- **VAE**: Flux VAE such as `ae.safetensors`
- Flux CLIPs `clip_l.safetensors` and `t5/google_t5-v1_1-xxl_encoderonly-fp8_e4m3fn.safetensors` must be in `clips`
- **Source image**: the current character avatar is the source image
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

### Replacements:

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

### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* DualCLIPLoader `clip_name1`: `"clip_l.safetensors"`
* DualCLIPLoader `clip_name2`: `"t5/google_t5-v1_1-x`
* DualCLIPLoader `type`: `"flux"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Resize image `mode`: `"resize"`
* Resize image `supersample`: `"true"`
* Resize image `resampling`: `"lanczos"`
* Resize image `rescale_factor`: `2`

## SE_SD_txt2img_v21

SD text to image with LoRA tag loading and optional background removal


### Files

* `SE_SD_txt2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_SD_txt2img_v21_ST.json`: Placeholder version for use in SillyTavern

### Notes

  - **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
  - **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

### Replacements:

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

### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Empty Latent Image `batch_size`: `1`

## SE_SD_img2img_v21

SD image to image with LoRA tag loading and optional background removal


### Files

* `SE_SD_img2img_v21.json`: Editable version for modifying in ComfyUI
* `SE_SD_img2img_v21_ST.json`: Placeholder version for use in SillyTavern

### Notes

- **Source image**: the current character avatar is the source image
- **LoRA tags**: specify LoRAs as tags in the prompt e.g. `<lora:LoRA_Filename:0.8>`
- **background removal**: add custom placeholder `%bgr_threshold%`, set between 0.0 (off) and about 0.5

### Replacements:

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

### Fixed input values:

* Save Image `filename_prefix`: `"SillyTavern"`
* Inspyrenet Rembg `torchscript_jit`: `"default"`
* Resize image `mode`: `"resize"`
* Resize image `supersample`: `"true"`
* Resize image `resampling`: `"lanczos"`
* Resize image `rescale_factor`: `2`
