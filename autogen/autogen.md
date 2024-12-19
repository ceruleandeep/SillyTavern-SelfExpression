# Sprite Autogen

This method uses FLUX to generate a new backgroundless sprite for each character message.

The easy way to do this is:

```
/sd quiet=true edit=false you |
/if left={{pipe}} {: /dom action=attribute attribute=src value={{pipe}} #expression-image :}
```

so if you're happy with the current results you get from `/sd you`, stick that in a QR and you're done.
It will generate a new sprite and swap it into place with [
`/dom`](https://github.com/LenAnderson/SillyTavern-LALib?tab=readme-ov-file#lalib-help-cmd-dom).

What I would be happier with is:

* the image shows the character's current emotion
* no nasty background
* the image is high quality and consistent with their usual sprites

So the finished QR will be

```
/* turn off the current spriteset or we will get flicker *|
/costume none |

/* update emotion classification. sometimes results in 2 classification runs... 
but if we don't, we may get 0 classification runs which sucks more *|
/classify {{lastCharMessage}} | /setvar key=lastsprite |

/* custom gen settings: remove background *|
/setvar key=bgr_threshold 0.5 |

/* generate with Schnell *|
/sd quiet=true edit=false model=flux1-schnell-Q4_K_S.gguf steps=4 you |

/* overwrite current sprite with generated image *|
/if left={{pipe}} {: /dom action=attribute attribute=src value={{pipe}} #expression-image :}
```

## Read the docs!

If you want to live on the edge, you gotta know where the edge is. These docs are good, they just got updated, there is
no excuse for not reading them:

* [ComfyUI configuration](https://docs.sillytavern.app/extensions/stable-diffusion/#comfyui-configuration)
* [Loading and editing ComfyUI workflows in ST](https://docs.sillytavern.app/extensions/stable-diffusion/#workflow-editor)
* [Generating images with slash commands](https://docs.sillytavern.app/extensions/stable-diffusion/#how-to-generate-an-image)

The following docs sections are older/sparser but still relevant:

* [Expression Images](https://docs.sillytavern.app/extensions/expression-images/)
* [Visual Novel mode](https://docs.sillytavern.app/usage/user-settings/visual-novel/)

These features of ST will be used without further explanation:

* [Installing custom extensions](https://docs.sillytavern.app/extensions/#extensions-panel)
* [Quick Replies](https://docs.sillytavern.app/usage/st-script/#quick-replies-script-library-and-auto-execution)
* [Variables](https://docs.sillytavern.app/usage/st-script/#variables)

## Brief setup:

Get [ComfyUI workflows](../workflows/workflows.md). Load [`SE_FLUX_txt2img_v21`](../workflows/SE_FLUX_txt2img_v21.json)
into ComfyUI and use Manager to install any custom nodes, models, VAEs, and CLIPs you don't have.

[`SE_FLUX_txt2img_v21_ST`](../workflows/SE_FLUX_txt2img_v21_ST.json) is for ST. Put that in `SillyTavern/data/default-user/user/workflows`.

In ST's Extensions Panel > Image Generation > ComfyUI:

- set Flux VAE and model
- edit `SE_FLUX_txt2img_v21_ST` workflow
- add `bgr_threshold` : `{{getvar::bgr_threshold}}`
- save
- add to "yourself" prompt something like:
  `{{char}}'s current facial expression is: {{getvar::lastsprite}}. ... must include physical features, facial expression, and appearance`

## TODO

* update and link to QRs