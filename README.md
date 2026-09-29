# Long-form AI Film / Series Starter

Безопасный скелет проекта для производства длинного AI-фильма или сериала через **Codex + ComfyUI Cloud + RunningHub**. Replicate не используется.

## Основной рекомендуемый pipeline

```text
CHARACTER BIBLE + LOCATION BIBLE + STYLE REFERENCES
→ Qwen Image 2.1 Image Edit
→ KEYFRAMES / STORYBOARD
→ MiniMax H3 Director (R2V / FL2V / RV2V)
→ 5–15 sec SHOTS
→ при сложном движении/актёрской игре: Wan 2.2 Animate
→ approved shots
→ SeedVR2
→ FINAL TIMELINE
→ EPISODE / FILM
```

Подробные этапы, развилки и критерии перехода: [`docs/PRODUCTION_PIPELINE.md`](docs/PRODUCTION_PIPELINE.md). Сводная таблица реальных графов, источников, совместимости и рисков: [`WORKFLOW_CATALOG.md`](WORKFLOW_CATALOG.md).

Ключевое правило экономии: **не апскейлить черновые takes**. Сначала шот получает статус `approved`, и только после этого его можно передавать в SeedVR2.

Реальные опубликованные workflow и точные источники перечислены в [`WORKFLOW_CATALOG.md`](WORKFLOW_CATALOG.md) и [`workflows/real_examples/`](workflows/real_examples/README.md). Файлы в [`workflows/examples/`](workflows/examples/README.md) — только **legacy/example**: это условные архитектурные схемы, не основной production path и не готовые графы для запуска.

## Быстрый старт

1. Заполнить `project.yaml`: формат, визуальные правила, backend policy и требования к финальному мастеру.
2. Добавить изображения персонажей в `characters/<character_id>/references/` и заполнить character bible.
3. Добавить изображения локаций в `locations/<location_id>/references/` и заполнить location bible.
4. Заполнить `episodes/episode_001.yaml`, затем создать scene/shot manifests по схеме из `docs/MANIFESTS.md`.
5. Подготовить keyframes через официальный Qwen Image 2.1 Image Edit baseline из `workflows/real_examples/qwen/`; community GGUF-вариант считать альтернативой с custom dependencies.
6. Генерировать 5–15-секундные шоты в MiniMax H3 Director через R2V, FL2V или RV2V; использовать сохранённые upstream JSON из `workflows/real_examples/h3_director/`. Для dialogue/character shots с voice refs есть отдельный hosted-only H3 Reference + Sound reference.
7. Подключать реальный Wan 2.2 Animate graph из `workflows/real_examples/wan22_animate/` только для шотов, где нужен motion/performance transfer или character replacement.
8. Утвердить лучший take; только затем использовать один из реальных SeedVR2 examples из `workflows/real_examples/seedvr2/` и выполнять финальную сборку по `PLAN.md`.

## Структура

```text
long-form-ai-film-series-starter/
├── project.yaml
├── README.md
├── PLAN.md
├── WORKFLOW_CATALOG.md       # проверенные источники, статусы и риски
├── characters/              # character bibles и референсы
├── locations/               # location bibles и референсы
├── episodes/                # episode manifests
├── scenes/                  # scene manifests и continuity notes
├── shots/                   # shot/take manifests
├── references/              # общие визуальные/жанровые референсы
├── workflows/real_examples/ # реальные JSON, upstream metadata и SOURCE.md
├── workflows/examples/      # legacy/example; условные неисполняемые схемы
├── prompts/                 # versioned prompt blocks
├── audio/                   # dialogue, music, SFX, stems
├── outputs/                 # keyframes, takes, approved, masters
├── logs/                    # decisions, QC, cost ledger
├── config/                  # backend policy и naming
└── docs/                    # схемы manifest-файлов
```

## Идентификаторы и имена файлов

- Episode: `ep001`
- Scene: `ep001_sc010`
- Shot: `ep001_sc010_sh020`
- Take: `ep001_sc010_sh020_tk003`
- Segment: `ep001_sc010_sh020_tk003_seg02`

Рекомендуемое имя результата:

```text
<shot_id>_<take_id>_<stage>_v###.<ext>
```

Например: `ep001_sc010_sh020_tk003_approved_v002.mp4`.

## Границы безопасности

- `execution.allow_generation` по умолчанию равен `false`.
- RunningHub — единственный разрешённый платный backend, способ оплаты: RH Coins.
- Любой платный запуск требует отдельного явного подтверждения пользователя.
- Перед запуском ComfyUI Cloud нужно проверить доступность nodes/models через live inventory/MCP.
- Перед запуском RunningHub нужно заменить placeholder workflow/app ID, проверить входы и оценку RH Coins.
- Статическая валидность JSON не доказывает доступность модели, custom nodes или успешный runtime.

## Что добавляет пользователь позже

- сюжет, сценарий и shot list;
- изображения и видео-референсы;
- утверждённые character/location bibles;
- реальные Cloud/RunningHub workflow IDs после ручного выбора и проверки;
- диалоги, озвучку, музыку и SFX;
- лимиты RH Coins и правила подтверждения запусков.

