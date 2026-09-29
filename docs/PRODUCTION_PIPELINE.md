# PRODUCTION PIPELINE — long-form AI film / series

Этот документ фиксирует основной рекомендуемый production path проекта. Структурная единица неизменна:

```text
Episode → Scene → Shot → Take
```

`Episode` задаёт драматургическую и delivery-единицу, `Scene` — место/время/continuity, `Shot` — одну монтажную задачу, `Take` — отдельную генерационную попытку. Для продолжения длинного take допускаются именованные segments, но они не заменяют эту иерархию.

## Главная цепочка

```text
CHARACTER BIBLE + LOCATION BIBLE + STYLE REFERENCES
                         │
                         ▼
              Qwen Image 2.1 Image Edit
                         │
                         ▼
                KEYFRAMES / STORYBOARD
                         │
                         ▼
                  MiniMax H3 Director
                 ┌───────┼───────┐
                 │       │       │
                R2V     FL2V    RV2V
                 └───────┼───────┘
                         ▼
                   5–15 sec SHOTS
                         │
             сложное движение / игра?
                   ┌─────┴─────┐
                 нет           да
                   │            ▼
                   │     Wan 2.2 Animate
                   └─────┬─────┘
                         ▼
                   approved shots
                         │
                         ▼
                       SeedVR2
                         │
                         ▼
                   FINAL TIMELINE
                         │
                         ▼
                   EPISODE / FILM
```

## Роли инструментов

### Qwen Image 2.1 Image Edit

Отвечает за внешний вид: character, location и style consistency, storyboard frames, start/end keyframes. Входы должны происходить из утверждённых character/location bibles и style references. Qwen не является основным видеогенератором.

Реальные JSON и источники: [`Qwen Image 2.1`](../workflows/real_examples/qwen/SOURCE.md).

### MiniMax H3 Director

Основной генератор шотов. Используется для 5–15-секундных клипов, multi-segment/timeline, continuity и selective rerender:

- `R2V` — новый шот из character/location/style references;
- `FL2V` — шот между утверждёнными первым и последним keyframe;
- `RV2V` — source video плюс дополнительные character/location/audio references.

Если один take или segment не прошёл QC, пересчитывается только он; утверждённые части сохраняются.

Реальные JSON и источники: [`MiniMax H3 Director`](../workflows/real_examples/h3_director/SOURCE.md).

### MiniMax H3 Reference + Sound

Специализированная ветка H3 для сложных dialogue/character shots с reference images и voice refs. Использовать, когда обычного H3 Director недостаточно для согласованности героя, одежды, сцены и голоса.

Реальный hosted-источник: [`H3 Reference + Sound`](../workflows/real_examples/h3_director/runninghub_h3_reference_to_video_sound.reference.md).

### Wan 2.2 Animate

Ветка для motion/performance transfer и character replacement. Подключается только при сложном движении или актёрской игре: походка, жесты, мимика, танец, драка, взаимодействие персонажей. Performance-video задаёт движение и тайминг; Wan не должен бесконтрольно изобретать новое действие вместо спланированного шота.

Реальный JSON и источник: [`Wan 2.2 Animate`](../workflows/real_examples/wan22_animate/SOURCE.md).

### SeedVR2

Финальный upscale только утверждённых шотов. SeedVR2 не используется для исправления плохой геометрии, continuity или актёрской игры — такой take возвращается на selective rerender.

Реальный источник: [`SeedVR2`](../workflows/real_examples/seedvr2/SOURCE.md).

## Gate экономии

```text
draft take → review → approved shot → SeedVR2
                  └→ rejected → selective rerender / новый take
```

Обязательное правило: **не апскейлить черновые takes**. SeedVR2 разрешён только после `approved`. Это исключает расход вычислений и RH Coins на материал, который всё равно будет отброшен.

## Что не является основным путём

`workflows/examples/*.example.json` — условные схемы раннего проектирования. Они сохранены как `legacy/example`, не являются проверенными production workflows и не должны запускаться как готовые графы. Для production-решений начинать с каталога [`workflows/real_examples/`](../workflows/real_examples/README.md), затем проверять актуальную доступность nodes/models на выбранном backend.

Документ описывает порядок производства, но не разрешает платные запуски. Генерации требуют отдельного явного подтверждения и live-проверки выбранного backend.
