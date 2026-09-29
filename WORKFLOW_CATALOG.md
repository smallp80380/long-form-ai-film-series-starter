# WORKFLOW CATALOG — verified public examples

Проверено **2026-09-29**. Каталог отделяет реально опубликованные артефакты от ранних условных схем. Валидный JSON означает только, что файл синтаксически читается; это не доказывает наличие nodes/models или успешный runtime на ComfyUI Cloud/RunningHub.

## Рекомендуемая производственная цепочка

```text
Qwen Image 2.1 keyframes
→ MiniMax H3 Director / R2V shot generation
→ Wan 2.2 Animate при необходимости performance/motion transfer
→ SeedVR2 только для approved takes
→ final edit
```

## Каталог

| Этап | Реальный источник | Локальные артефакты | Cloud | RunningHub | Основной риск |
|---|---|---|---|---|---|
| Keyframe / image edit | Comfy-Org official Qwen 2.1 template | `qwen/image_qwen_image_2_1_image_edit.json` | likely, не подтверждено | unknown | packaged subgraph и модели должны существовать в runtime |
| Keyframe / T2I reference | foprc Qwen 2.1 GGUF | `qwen/qwen_image_2_1_Q8_0_t2i.json` | unknown | unknown | ComfyUI-GGUF, rgthree, ComfyUI 0.37+ |
| Long-form / multi-segment H3 | AIMixer MiniMaxH3 Director + RH Station/Desk | 5 JSON: t2v, fl2v, r2v, v2v, rv2v | unknown | confirmed hosted pages | custom node/version, H3 weights, non-deterministic continuity |
| H3 reference + sound | Prompt Mastery, RunningHub | reference `.md` only; raw JSON не подтверждён | unknown | confirmed hosted page | custom dual-clock nodes; проверить coin estimate |
| Timeline/compiler alternative | imbutus MiniMaxDirector | `h3_director/minimax_director_timeline_compiler.json` | unknown | unknown | отдельная реализация, не смешивать с AIMixer nodes |
| Performance / character transfer | rik-python Wan 2.2 Animate | `wan22_animate/YT-Wan2.2-Anim-SwapAnything-v01.json` | unknown | unknown | 102-node graph, много custom packs/models, нет LICENSE-файла |
| Postprocess | itontonpi SeedVR2 fork | 3 JSON для image/HD video/4K image | unknown | unknown | тяжёлый custom runtime; fork, не upstream |

## Первичные источники

### Qwen Image 2.1

- Community: https://github.com/foprc/qwen-image-2.1-comfyui-workflow
- Official baseline: https://github.com/Comfy-Org/workflow_templates/blob/main/templates/image_qwen_image_2_1_image_edit.json
- Метаданные и требования: [`workflows/real_examples/qwen/SOURCE.md`](workflows/real_examples/qwen/SOURCE.md)

### MiniMax H3 Director

- RunningHub Director Station: https://www.runninghub.ai/post/2084945577214124033
- RunningHub Director's Desk: https://www.runninghub.ai/post/2094582511883345922
- Public upstream JSON: https://github.com/AIMixer/ComfyUI_MiniMaxH3_Director/tree/main/example_workflows
- Проверенный независимый timeline compiler: https://github.com/imbutus/ComfyUI-MiniMaxDirector
- Метаданные и требования: [`workflows/real_examples/h3_director/SOURCE.md`](workflows/real_examples/h3_director/SOURCE.md)

### MiniMax H3 Reference to Video + Sound

- RunningHub: https://www.runninghub.ai/post/2091751881252093954
- Локально сохранено только проверяемое описание: [`runninghub_h3_reference_to_video_sound.reference.md`](workflows/real_examples/h3_director/runninghub_h3_reference_to_video_sound.reference.md)

### Wan 2.2 Animate

- Repository: https://github.com/rik-python/AI-CharacterSwap-Workflow
- Метаданные и требования: [`workflows/real_examples/wan22_animate/SOURCE.md`](workflows/real_examples/wan22_animate/SOURCE.md)

### SeedVR2

- Repository: https://github.com/itontonpi/seedvr2
- Метаданные и требования: [`workflows/real_examples/seedvr2/SOURCE.md`](workflows/real_examples/seedvr2/SOURCE.md)

## Платёжная граница

- Replicate не используется.
- Для RunningHub допустима только оплата RH Coins.
- Статус `RunningHub confirmed` означает наличие опубликованной hosted page, а не разрешение на запуск.
- До каждого RH run: проверить nodes/models, входы, coin estimate и отсутствие external API-node/отдельной подписки.
- В этой задаче генерации не запускались.

## Legacy

`workflows/examples/*.example.json` — условные схемы и не должны выдаваться за готовые графы. Старые reference-only папки с длинными именами внутри `workflows/real_examples/` сохранены для обратных ссылок; актуальные артефакты и полные `SOURCE.md` находятся в `qwen/`, `h3_director/`, `wan22_animate/`, `seedvr2/`.

