# Real workflow examples — source catalog

Этот каталог хранит локальные копии реально опубликованных workflow там, где публичный JSON доступен, и проверяемые source references для hosted workflow. Наличие файла не означает, что workflow уже установлен или совместим с текущим backend: перед запуском всё равно нужна live-проверка nodes, models, входов и стоимости.

Контрольные суммы всех сохранённых JSON: [`CHECKSUMS.sha256`](CHECKSUMS.sha256).

| Роль | Локальный SOURCE.md | Статус артефакта |
|---|---|---|
| Qwen Image 2.1 Image Edit | [`qwen/SOURCE.md`](qwen/SOURCE.md) | официальный и community JSON сохранены локально |
| MiniMax H3 Director | [`h3_director/SOURCE.md`](h3_director/SOURCE.md) | пять Director JSON и независимый timeline example сохранены локально |
| MiniMax H3 Reference + Sound | [`h3_director/runninghub_h3_reference_to_video_sound.reference.md`](h3_director/runninghub_h3_reference_to_video_sound.reference.md) | RunningHub-hosted workflow; raw JSON локально не сохранён |
| Wan 2.2 Animate | [`wan22_animate/SOURCE.md`](wan22_animate/SOURCE.md) | upstream JSON сохранён локально |
| SeedVR2 | [`seedvr2/SOURCE.md`](seedvr2/SOURCE.md) | три example JSON сохранены локально |

Полная матрица совместимости и рисков: [`../../WORKFLOW_CATALOG.md`](../../WORKFLOW_CATALOG.md). Основной порядок применения: [`../../docs/PRODUCTION_PIPELINE.md`](../../docs/PRODUCTION_PIPELINE.md).

Папки `qwen_image_2_1_image_edit/`, `minimax_h3_director/`, `minimax_h3_reference_sound/` и `wan_2_2_animate/` оставлены как старые reference-only пути для обратных ссылок. Актуальные файлы находятся в четырёх каталогах из таблицы выше.
