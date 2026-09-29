# SOURCE — Qwen Image 2.1

## 1. Community workflow: Qwen Image 2.1 GGUF Q8_0

- **Название:** `qwen_image_2_1_Q8_0_t2i.json`
- **Автор / репозиторий:** foprc, `qwen-image-2.1-comfyui-workflow`
- **Прямая ссылка:** https://github.com/foprc/qwen-image-2.1-comfyui-workflow/blob/master/workflows/qwen_image_2_1_Q8_0_t2i.json
- **Зафиксированная версия:** commit `e2d9dcf6d993bd34f083c6536ef2f0f4568e2512`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** публичный UI-format JSON, README и MIT license; локальная копия JSON и upstream README сохранены рядом.
- **Задача:** text-to-image с опциональной depth/reference-веткой и тремя слотами LoRA.
- **Почему полезен для фильма/сериала:** быстрый источник keyframes, вариантов композиции и визуального look-dev перед анимацией шота.
- **ComfyUI Cloud compatibility:** **unknown**. Граф требует ComfyUI 0.37.0+, `ComfyUI-GGUF` и `rgthree-comfy`; наличие этих пакетов и моделей в Cloud не подтверждено.
- **RunningHub compatibility:** **unknown**. Нужна проверка наличия custom nodes и точных model filenames в конкретном RH environment.
- **Внешние API / дополнительная оплата:** внешние API в графе не обнаружены. Требуются модели; на RunningHub перед запуском всё равно проверить, что расчёт идёт только за RH Coins и нет платного API-node.
- **Custom nodes / модели:** `UnetLoaderGGUF` из ComfyUI-GGUF; `Power Lora Loader (rgthree)`; `qwen-image-2.1-Q8_0.gguf`, `qwen3vl_8b_int8_convrot.safetensors`, `qwen_image_2.1_vae_bf16.safetensors`.
- **Зрелость / риски:** небольшой community repo; workflow построен под свежие core nodes. При старой версии ComfyUI возможен неверный latent scale. Это не официальный Cloud template.

## 2. Official Comfy-Org baseline: Qwen Image 2.1 image edit

- **Название:** `image_qwen_image_2_1_image_edit.json`
- **Автор / репозиторий:** Comfy-Org, `workflow_templates`
- **Прямая ссылка:** https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_qwen_image_2_1_image_edit.json
- **Зафиксированная версия:** ветка `main`, commit `dd9769b10b2df75769bd32e349dc0d7927f2a705`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** официальный UI-format template JSON; локальная копия сохранена рядом.
- **Задача:** image edit с `image_1` как целью и до девяти дополнительных reference images; подходит для смены костюма, окружения и сборки keyframe.
- **Почему полезен для фильма/сериала:** более уместный baseline для production keyframes, чем чистый T2I: можно редактировать утверждённый кадр и сохранять композиционную основу.
- **ComfyUI Cloud compatibility:** **likely, not confirmed**. Это официальный Comfy-Org template, но сам template предупреждает, что Desktop/Cloud могут отставать от nightly; перед запуском требуется live inventory.
- **RunningHub compatibility:** **unknown**. Требуется наличие официального Qwen 2.1 subgraph/template и моделей.
- **Внешние API / дополнительная оплата:** внешние API в JSON не обнаружены. На RH запуск допустим только после подтверждения расчёта исключительно в RH Coins.
- **Custom nodes / модели:** template использует packaged subgraph с UUID type; требуются `qwen_image_2.1_int8_convrot.safetensors`, `qwen3vl_8b_int8_convrot.safetensors`, `qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors`, `qwen_image_2.1_vae_bf16.safetensors`.
- **Зрелость / риски:** официальный baseline, но это UI graph, не API prompt JSON. Без соответствующей версии ComfyUI и зарегистрированного subgraph узел откроется как missing.

