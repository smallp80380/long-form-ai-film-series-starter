# PLAN — архитектура long-form производства

## 0. Основной production path

Единственный рекомендуемый путь проекта:

```text
CHARACTER BIBLE + LOCATION BIBLE + STYLE REFERENCES
→ Qwen Image 2.1 Image Edit
→ KEYFRAMES / STORYBOARD
→ MiniMax H3 Director
   ├── R2V
   ├── FL2V
   └── RV2V
→ 5–15 sec SHOTS
→ при сложном движении/актёрской игре: Wan 2.2 Animate
→ approved shots
→ SeedVR2
→ FINAL TIMELINE
→ EPISODE / FILM
```

Исполнительные правила и ссылки на реальные workflow: [`docs/PRODUCTION_PIPELINE.md`](docs/PRODUCTION_PIPELINE.md) и [`WORKFLOW_CATALOG.md`](WORKFLOW_CATALOG.md). Условные JSON из `workflows/examples/` имеют статус `legacy/example` и не являются production path.

## 1. Производственная единица

Проект строится как иерархия:

```text
Series / Film
└── Episode
    └── Scene
        └── Shot
            └── Take
                └── Segment (при продолжении длинного шота)
```

**Episode** задаёт драматургическую дугу и delivery. **Scene** фиксирует место, время, свет, персонажей и continuity. **Shot** содержит одну монтажную задачу и камеру. **Take** — конкретная генерационная попытка с неизменяемыми параметрами и seed. **Segment** — часть take, продолжаемая от последнего утверждённого кадра/контекста.

## 2. Источники правды

1. `project.yaml` — глобальная политика, стиль, backend и delivery.
2. `characters/**/character.yaml` — внешность, костюм, голос, поведение, запрещённые изменения.
3. `locations/**/location.yaml` — геометрия, свет, палитра, постоянные предметы.
4. `episodes/*.yaml` — сюжетная структура и статус производства.
5. `scenes/**/*.yaml` — continuity сцены.
6. `shots/**/*.yaml` — camera/action/take/segment параметры.
7. `logs/decisions.md` — причины утверждения/отклонения результатов.

Новый take никогда не перезаписывает предыдущий. Изменение character/location bible требует пометки затронутых будущих и уже созданных шотов.

## 3. Этапы производства

### A. Development и continuity

- Заполнить logline, synopsis, episode beats.
- Создать character и location bibles.
- Зафиксировать aspect ratio, fps, разрешение, цвет и правила камеры.
- Разбить Episode → Scene → Shot; определить длительность и монтажную функцию каждого шота.

### B. Keyframe / reference preparation — ComfyUI Cloud

- Входы: character/location/prop references, композиционный sketch, опциональные pose/depth/edge maps.
- Основной инструмент: Qwen Image 2.1 Image Edit для внешнего вида, character/location/style consistency и сборки master keyframes.
- Выходы: `keyframe_start`, опционально `keyframe_end`, reference sheet и QC metadata.
- Начинать с official baseline `workflows/real_examples/qwen/image_qwen_image_2_1_image_edit.json`; community GGUF workflow использовать только после проверки ComfyUI-GGUF/rgthree и версии core.
- До любого запуска: live-проверка nodes/models через MCP; валидный JSON сам по себе не подтверждает runtime compatibility.

### C. Shot generation — RunningHub / MiniMax H3 Director style

- MiniMax H3 Director — основной генератор 5–15-секундных шотов: `R2V`, `FL2V`, `RV2V`, multi-segment/timeline, continuity и selective rerender. Реальные AIMixer JSON и отдельный проверенный timeline compiler лежат в `workflows/real_examples/h3_director/`.
- MiniMax H3 Reference + Sound — специальная ветка для сложных dialogue/character shots, где нужны reference images и voice refs.
- Каждый shot генерируется как один или несколько именованных segments.
- Следующий segment получает continuation anchor: последний утверждённый кадр предыдущего сегмента, те же bibles, camera state и continuity prompt.
- `selective_rerender` меняет только провалившийся segment/take; утверждённые части сохраняются.
- Платный запуск разрешён только после оценки RH Coins и отдельного подтверждения.

### D. Performance / character replacement / motion transfer — Wan2.2 Animate concept

- Wan 2.2 Animate — не обязательный этап каждого шота, а ветка для сложного движения, актёрской игры, motion/performance transfer и character replacement. Реальный community graph сохранён как `workflows/real_examples/wan22_animate/YT-Wan2.2-Anim-SwapAnything-v01.json`; перед импортом учитывать большой набор custom dependencies.
- Performance-video задаёт существующие движение, ритм и блокинг.
- Character reference задаёт целевую идентичность/костюм.
- Pose/depth/segmentation помогают отделить движение, геометрию и области замены.
- Motion transfer не считается способом изобрести новое действие. Для нового действия нужен новый performance source либо отдельный planned shot с start/end keyframes.

### E. QC и selective rerender

Для каждого take проверить:

- identity и costume consistency;
- геометрию рук/лица/предметов;
- continuity направления взгляда, screen direction и света;
- temporal artifacts, flicker, morphing;
- соответствие длительности и монтажной функции;
- синхронизацию аудио, если оно есть.

Статус: `draft → review → approved` либо `rejected`. Причина rejection записывается в take manifest, чтобы следующий запуск менял только нужные параметры.

### F. Upscale и interpolation

1. Сначала утвердить монтаж и содержание на proxy/draft качестве.
2. **Запрещено апскейлить draft/review takes.** Сначала `approved`, затем SeedVR2.
3. SeedVR2 — финальный upscale только утверждённых шотов; он не заменяет rerender дефектного take. Использовать один из трёх опубликованных JSON в `workflows/real_examples/seedvr2/` только после проверки custom node/models.
4. Interpolation выполнять после стабилизации и deflicker; хранить исходный native fps.
5. Проверить лица, быстрые движения, motion blur и синхронизацию после обработки.
6. Не выдавать post-process за исправление сломанной геометрии — такой shot отправлять на rerender.

### G. Финальная сборка

- Скопировать approved takes в `outputs/approved/` без изменения исходников.
- Собрать scene timelines, затем episode timeline.
- Выровнять fps, resolution, color space и audio sample rate.
- Добавить dialogue, room tone, SFX, music stems и loudness pass.
- Экспортировать review master и после QC — delivery master.
- Сохранить assembly manifest с точными входными версиями.

## 4. Backend policy

| Этап | Backend | Оплата | Правило |
|---|---|---|---|
| Keyframes / edit | ComfyUI Cloud | по текущим условиям аккаунта | Никаких предположений о nodes/models; сначала live inventory |
| Long-form H3 shots | RunningHub | только RH Coins | Estimate + explicit confirmation перед запуском |
| Wan performance transfer | ComfyUI Cloud или RunningHub | RunningHub только RH Coins | Сначала проверить реальный template/node availability |
| Assembly | локально/монтажная система | без внешнего API | Работать только с утверждёнными файлами |

Replicate запрещён в `project.yaml` и не предусмотрен workflow-примерами.

## 5. Версионирование и воспроизводимость

Каждый take хранит:

- prompt version и negative constraints;
- input asset hashes/версии;
- backend и workflow revision;
- seed, если backend его предоставляет;
- model/node inventory snapshot;
- RH Coins estimate/actual (для RunningHub);
- timestamps, QC decision и parent take/segment.

Seed помогает повторяемости, но не обеспечивает continuity сам по себе; continuity опирается на утверждённые anchors и неизменяемые bibles.

