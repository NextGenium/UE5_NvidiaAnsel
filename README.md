# Nvidia Ansel

> **📖 Upstream documentation — MonsterGuo / UE5_NvidiaAnsel**
>
> Базовая архитектура (Nvidia Ansel photo-mode для UE5, freeze game / camera control / filters / screenshot capture, в том числе 360° и stereo 360° рендер) — в upstream repo: **https://github.com/MonsterGuo/UE5_NvidiaAnsel** (README на китайском, с пошаговой инструкцией по настройке GeForce Experience и viewport).
>
> Этот README описывает плагин на high-level + **NextGenium-доработки** относительно upstream. Для оригинальной документации Epic Games `Ansel` plugin см. UE Engine source — `Engine/Plugins/Runtime/Nvidia/Ansel`.

## Overview

Photo-mode плагин на основе Nvidia Ansel SDK для UE5. Позволяет в runtime заморозить игру, управлять камерой, накладывать фильтры и делать высококачественные screenshots (включая 360° и stereo 360°). Это портация upstream `MonsterGuo/UE5_NvidiaAnsel` (UE4.27 → UE5), который сам является доработкой оригинального Epic Games `Ansel` плагина.

В Next Framework входит как `3rdParty` плагин (Category: `Photography`). Полезен для маркетинга и PR-материалов проектов — генерация screenshots в высоком разрешении без отдельного toolset'а.

## When to use

- Нужен встроенный photo-mode без написания собственной capture-системы.
- Команде маркетинга / арт-направления нужны 360° и stereo 360° screenshots проекта.
- Нужно дать игрокам встроенную возможность делать качественные screenshots с фильтрами.
- Проект на UE5.6+ и нужна портированная версия Epic'овского `Ansel` плагина (оригинал был UE4-only).

## Boundary

- **Видео-захват / replay** — Ansel делает только static screenshots; для видео нужен Movie Render Queue / OBS.
- **Post-processing pipeline** — Ansel работает поверх существующего PP, не заменяет его.
- **Запись gameplay session** — не входит в scope, только моментальные кадры.
- **Поддержка не-Nvidia GPU** — Ansel SDK работает только с Nvidia GeForce / RTX картами.

## NextGenium-доработки (поверх upstream)

NextGenium-доработок поверх upstream на момент написания не зафиксировано. Repo — clean fork от `MonsterGuo/UE5_NvidiaAnsel` (branch `UE5.6`).

## Modules

| Модуль | Тип | LoadingPhase | Назначение |
|---|---|---|---|
| `Ansel` | Runtime | `PostConfigInit` | Интеграция Nvidia Ansel SDK в UE renderer (camera control, screenshot capture, filter overlay). |

**Plugin dependencies:** нет (только Nvidia Ansel SDK binaries).
**Platform:** `Win64` (по `.uplugin`).

## Installation

Через Next Framework Loader: **Nvidia Ansel**.

Или вручную:

```bash
cd <YourProject>/Plugins
git clone https://github.com/NextGenium/UE5_NvidiaAnsel.git
```

В корне репо лежит `NvCameraConfiguration.exe` — конфигуратор для расширенных настроек Ansel (resolution presets и т.п.).

## How to use (high-level)

1. Скопировать плагин в `<YourProject>/Plugins/Ansel`.
2. В проекте включить плагин **Nvidia Ansel Photography Plugin** в Plugin Settings.
3. На целевой машине должны быть установлены актуальные Nvidia GeForce drivers (тестируются 456.71 — 551.79) и GeForce Experience (для управления сохранением кадров).
4. Перед использованием в Nvidia overlay отключить «In-Game Overlay» — иначе Ansel не запускается из движка (см. upstream README, шаги (2)–(5)).
5. В сцене отключить auto-exposure, использовать 16:9 viewport (1920x1080 / 2560x1440 / 3840x2160), запустить Ansel hotkey'ем — управлять камерой / фильтрами / capture через встроенный Ansel UI.

Детальный setup-guide со скриншотами — в upstream README.

## TODO

- Carry-over from upstream: см. [issues MonsterGuo/UE5_NvidiaAnsel](https://github.com/MonsterGuo/UE5_NvidiaAnsel/issues).
- NextGenium-specific: TBD — добавится по мере использования.

## Limitations

- `PlatformAllowList: ["Win64"]` в `.uplugin` — для других платформ нужна явная правка (Ansel SDK сам по себе Win-only).
- Работает только с Nvidia GeForce / RTX GPU — на AMD/Intel плагин не активируется.
- Требует GeForce Experience для управления save-path; без него screenshots идут в системную папку «Видео».
- Upstream README на китайском; setup steps требуют перевода для русскоязычной команды.

## Origin

Upstream — **`MonsterGuo/UE5_NvidiaAnsel`** (https://github.com/MonsterGuo/UE5_NvidiaAnsel), license не объявлена, портация оригинального Epic Games `Ansel` плагина с UE4.27 на UE5 автором **MonsterGuo**. В `.uplugin` `CreatedBy: "Epic Games, Inc."` (унаследовано от оригинала).

NextGenium-форк используется как-есть (branch `UE5.6`) — см. секцию «NextGenium-доработки».

## Maintainers

TBD. На момент написания не зафиксирован — открытый вопрос к Диме.

## References

- **Upstream repo:** https://github.com/MonsterGuo/UE5_NvidiaAnsel
- **Nvidia Ansel SDK:** https://developer.nvidia.com/rtx/ansel
- **Epic Games Ansel plugin (original):** в UE source — `Engine/Plugins/Runtime/Nvidia/Ansel`.
