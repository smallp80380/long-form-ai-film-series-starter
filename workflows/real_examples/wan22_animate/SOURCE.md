# SOURCE — Wan 2.2 Animate character/performance transfer

- **Название:** `YT-Wan2.2-Anim-SwapAnything-v01.json`
- **Автор / репозиторий:** rik-python, `AI-CharacterSwap-Workflow`
- **Прямая ссылка:** https://github.com/rik-python/AI-CharacterSwap-Workflow/blob/main/YT-Wan2.2-Anim-SwapAnything-v01.json
- **Зафиксированная версия:** commit `6663241f450981ff48fdccffca2d416311b9af5e`
- **Дата проверки:** 2026-09-29
- **Что реально есть:** публичный UI-format JSON на 102 nodes и README; локальная копия обоих файлов сохранена рядом. В репозитории не найден отдельный LICENSE-файл.
- **Задача:** face/head/full-character replacement с сохранением performance source, авто-ротоскопированием, pose tracking и face locking.
- **Почему полезен для фильма/сериала:** позволяет переносить утверждённую игру/блокинг на персонажа, делать previs, wardrobe/prop replacement и временные VFX-компы.
- **ComfyUI Cloud compatibility:** **unknown**. Граф зависит от большого набора custom nodes и локальных моделей; готовность Cloud не подтверждена.
- **RunningHub compatibility:** **unknown**. Импорт может открыть missing nodes; наличие полного набора пакетов и моделей на RH не проверено.
- **Внешние API / дополнительная оплата:** узел `AILab_QwenVL` настроен на `Qwen3-VL-2B-Instruct`, но из JSON нельзя надёжно определить способ получения весов/вызова. До RH-запуска обязательно подтвердить, что он локальный и что нет отдельной API-оплаты сверх RH Coins.
- **Custom nodes / модели:** ComfyUI-WanVideoWrapper, ComfyUI-WanAnimatePreprocess, SAM3 nodes, FantasyPortrait, VideoHelperSuite, KJNodes, rgthree, pysssss и другие; среди явных моделей — `Wan2_2-Animate-14B_fp8_scaled_e4m3fn_KJ_v2.safetensors`, `Wan2_1_FantasyPortrait_fp16.safetensors`, `sam3.pt`, `vitpose-l-wholebody.onnx`, `yolov10m.onnx`.
- **Зрелость / риски:** production-oriented community graph, но upstream прямо не рекомендует его как final-pixel замену композитинга. Высокая сложность зависимостей, большой VRAM demand, риски на быстрых движениях и motion blur. Отсутствие LICENSE означает, что права на перераспространение/production reuse нужно уточнить у автора; копия сохранена как исследовательский reference.

