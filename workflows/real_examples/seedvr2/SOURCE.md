# SOURCE — SeedVR2 postprocess workflows

- **Название:** `SeedVR2_simple_image_upscale.json`, `SeedVR2_HD_video_upscale.json`, `SeedVR2_4K_image_upscale.json`
- **Автор / репозиторий:** itontonpi, `seedvr2`; репозиторий сам обозначает себя как unofficial personal test fork от `numz/ComfyUI-SeedVR2_VideoUpscaler`.
- **Прямая ссылка:** https://github.com/itontonpi/seedvr2/tree/main/example_workflows
- **Зафиксированная версия:** commit `8cfe3390e09ad5faaad2555b78dcf9477db50dba`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** три публичных UI-format JSON, preview images, README и Apache-2.0 license; JSON, README и license сохранены рядом.
- **Задача:** image/video upscale через SeedVR2 3B/7B с VAE tiling, offload, BlockSwap и optional compile.
- **Почему полезен для фильма/сериала:** postprocess только утверждённых takes: HD video upscale, 4K still/keyframe upscale и безопасная отдельная стадия после монтажа содержания.
- **ComfyUI Cloud compatibility:** **unknown**. Требуется custom-node repo, CUDA/MPS-oriented runtime и тяжёлые модели; inventory Cloud не проверен.
- **RunningHub compatibility:** **unknown**. Наличие `SeedVR2*` nodes и моделей на RH не подтверждено.
- **Внешние API / дополнительная оплата:** workflow работает на моделях, внешние API nodes не обнаружены. Loader умеет auto-download известных моделей, что является сетевой загрузкой, но не отдельным inference API. На RH до запуска проверить, что расходы ограничены RH Coins.
- **Custom nodes / модели:** сам repo `itontonpi/seedvr2`; `SeedVR2LoadDiTModel`, `SeedVR2LoadVAEModel`, `SeedVR2TorchCompileSettings`, `SeedVR2VideoUpscaler`; примеры используют `seedvr2_ema_3b_fp8_e4m3fn.safetensors`, `seedvr2_ema_3b_fp16.safetensors`, `seedvr2_ema_7b_sharp_fp16.safetensors`, `ema_vae_fp16.safetensors`.
- **Зрелость / риски:** fork, а не upstream; CUDA-heavy, высокие требования к памяти и времени. Upscale не исправляет сломанную геометрию/identity и может добавлять детали — нужен покадровый QC.

