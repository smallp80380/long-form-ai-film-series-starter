# RunningHub reference — MiniMax H3 Reference to Video + Sound reference V1

- **URL:** https://www.runninghub.ai/post/2091751881252093954
- **Автор / источник:** RunningHub creator `Prompt Mastery` (account id `2072160124218466306`).
- **Проверено:** 2026-09-29.
- **Статус:** только RunningHub-hosted workflow/reference; надёжная публичная raw JSON-ссылка вне RH не подтверждена, поэтому JSON не выдуман и не сохранён.
- **Inputs:** три изображения — face, outfit, scene — плюс два voice references и prompt.
- **Pipeline:** Dual-Clock sampler (`dual_clock_euler`) разводит video/audio schedules; stage 1 около 0.8 MP, stage 2 upscale около 1.3 MP. Страница указывает video shift 12, audio shift 3 и Ref2V Turbo 4-step как один из вариантов.
- **Почему полезен:** shot-level R2V с character/outfit/scene/voice references и отдельной заботой о разборчивости аудио при малом числе steps.
- **Совместимость:** RunningHub **confirmed** как hosted page; ComfyUI Cloud **unknown** из-за custom nodes (`MiniMaxH3DualClockSamplerT8`, `MiniMaxH3AVDecodeT8`, KJNodes, rgthree и др.).
- **Оплата:** node list не показывает отдельного внешнего inference API. До запуска подтвердить RH Coins estimate и отсутствие дополнительной оплаты за выбранные модели/nodes.

