# SillyTavern-SelfExpression

Experiments in self-selection and self-generation of agent images

## ComfyUI workflows for SillyTavern

* Flux txt2img: GGUF-quantized Flux text to image with LoRA tag loading and optional background removal [for ComfyUI](workflows/SE_FLUX_txt2img_v21.json), [for ST](workflows/SE_FLUX_txt2img_v21_ST.json)
* Flux img2img: GGUF-quantized Flux image to image with LoRA tag loading and optional background removal [for ComfyUI](workflows/SE_FLUX_img2img_v21.json), [for ST](workflows/SE_FLUX_img2img_v21_ST.json)
* SD txt2img: SD text to image with LoRA tag loading and optional background removal [for ComfyUI](workflows/SE_SD_txt2img_v21.json), [for ST](workflows/SE_SD_txt2img_v21_ST.json)
* SD img2img: SD image to image with LoRA tag loading and optional background removal [for ComfyUI](workflows/SE_SD_img2img_v21.json), [for ST](workflows/SE_SD_img2img_v21_ST.json)

See: [workflows](workflows/workflows.md)

## Comfier Placeholders

[SillyTavern-ComfierPlaceholders](https://github.com/ceruleandeep/SillyTavern-ComfierPlaceholders) automatically inserts
placeholders into ComfyUI workflows, to connect them to the SillyTavern image generation controls.
