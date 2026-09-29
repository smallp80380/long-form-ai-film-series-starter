# SOURCE — MiniMax H3 Director / timeline workflows

## A. AIMixer multi-segment Director (основной production reference)

- **Название:** MiniMax H3 Director Station / Director's Desk; локальные файлы `minimax_h3_director_{t2v,fl2v,r2v,v2v,rv2v}.json`.
- **Автор / репозиторий:** AI Mixer / AIMixer, https://github.com/AIMixer/ComfyUI_MiniMaxH3_Director
- **RunningHub pages:** https://www.runninghub.ai/post/2084945577214124033 и https://www.runninghub.ai/post/2094582511883345922
- **Зафиксированная версия:** commit `5c7bdc85fcc7e35849a89d0ba805299801fc6282`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** публичный custom-node repo, README, Apache-2.0 license и реальные UI-format JSON. Пять базовых JSON, upstream README и license сохранены рядом. RunningHub pages также существуют и ссылаются на этот repo.
- **Задача:** multi-segment/timeline H3 production: T2V, first/last-frame, reference-to-video, source-video editing и reference-guided source editing; Run Select позволяет пересчитывать выбранные группы.
- **Почему полезен для фильма/сериала:** это главный пример для `Episode → Scene → Shot → Take → Segment`: разделяет режимы по типу шота, поддерживает переход от предыдущего сегмента, материал-группы, исходный звук и selective rerender.
- **ComfyUI Cloud compatibility:** **unknown**. Требуются свежий ComfyUI с official MiniMax H3 nodes, custom node `AIMixer/ComfyUI_MiniMaxH3_Director` и H3 weights; Cloud inventory не проверен.
- **RunningHub compatibility:** **confirmed for the published hosted pages**. Импорт локальных JSON в другой RH workspace всё равно требует наличия соответствующего node pack/version.
- **Внешние API / дополнительная оплата:** в пяти сохранённых базовых графах нет отдельного external inference API-node. Для RH это кандидат на работу только за RH Coins; перед запуском обязательно проверить estimate и экран зависимостей.
- **Custom nodes / модели:** `MiniMaxH3Director`; ComfyUI ≥ 0.30.0; fl2va UNET для t2v/i2v/fl2v, ref2va UNET для r2v/v2v/rv2v; Qwen3-VL CLIP; video/audio VAE. Optional dependencies включают PySceneDetect/OpenCV/imageio-ffmpeg; refine/upscale расширения могут добавлять ещё модели и зависимости.
- **Зрелость / риски:** активно меняющийся plugin; длинная сцена всё равно состоит из нескольких генераций, поэтому continuity не детерминирована. Selective Run снижает стоимость ретейков, но cache/source passthrough и версия node pack должны быть зафиксированы в manifest.

## B. Prompt Mastery R2V + sound reference

- **Источник:** https://www.runninghub.ai/post/2091751881252093954
- **Что реально есть:** hosted workflow page и node/model list; raw JSON не сохранён. Подробности — `runninghub_h3_reference_to_video_sound.reference.md`.
- **Роль:** shot workflow с face/outfit/scene, двумя voice refs, dual-clock sampler и двухстадийным upscale.
- **Совместимость:** RunningHub confirmed; ComfyUI Cloud unknown.

## C. Проверенный независимый MiniMaxDirector timeline compiler

- **Название:** `minimax_director_timeline_compiler.json` (upstream filename `example_workflows/minimax-director.json`)
- **Автор / репозиторий:** imbutus, https://github.com/imbutus/ComfyUI-MiniMaxDirector
- **Зафиксированная версия:** commit `dc38ef9c5ec9f905e81d548fa14ec7aff8f92bfd`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** полноценный UI-format JSON, README и MIT license; локальные копии сохранены рядом.
- **Задача:** timeline editor/compiler, который формирует структурированный H3 prompt, валидирует reference limits/frame lattice и соединяет sampler с video/audio decode.
- **Почему полезен:** альтернативный, более prompt-compiler-oriented reference для shot/camera/audio timeline и linting до дорогого запуска.
- **ComfyUI Cloud compatibility:** **unknown**; нужен custom node и ComfyUI ≥ 0.31.0 для workflow с Turbo switch.
- **RunningHub compatibility:** **unknown**; отдельной подтверждённой hosted page для этой точной реализации не проверено.
- **Внешние API / дополнительная оплата:** external inference API не заявлен; используются локальные H3 weights. Optional Turbo LoRA и latent upscaler добавляют модели/пакет.
- **Custom nodes / модели:** `ComfyUI-MiniMaxDirector`; H3 Ref2VA UNET, Qwen3-VL text encoder, video/audio VAE; optional `Comfyui_Minimax_h3_latent_Upscaler`.
- **Зрелость / риски:** независимый проект, не тот же код, что AIMixer. Не смешивать JSON и node packs между реализациями. Никакой непроверенный проект под названием “H3 OBVPM” в каталог не добавлен.

