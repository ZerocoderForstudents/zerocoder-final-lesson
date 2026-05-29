---
name: zerocoder-presentation
description: Помогает работать с репозиторием zerocoder-final-lesson — HTML-презентацией финального урока курса Zerocoder. Используй когда нужно понять структуру, запустить, изменить слайды, обновить README или опубликовать на GitHub.
---

# Zerocoder Final Lesson — Presentation

## Проект

Интерактивная веб-презентация финального урока курса **Zerocoder** (Claude Code). 10 слайдов: от выбора идеи до публикации на GitHub. Без зависимостей — один HTML-файл плюс папка с изображениями.

**GitHub:** https://github.com/ZerocoderForstudents/zerocoder-final-lesson  
**Аккаунт:** ZerocoderForstudents  
**Email:** kv.zayceva@gmail.com

---

## Структура репозитория

```
fINAL/
├── README.md                        # Описание проекта
├── presentation/
│   ├── index.html                   # Вся презентация (единый файл)
│   └── images/                      # Фоновые изображения слайдов
│       ├── hero.png                 # Слайды 1 и 10
│       ├── why_project.png          # Слайд 2
│       ├── choose_idea.png          # Слайд 3
│       ├── describe_project.png     # Слайды 4 и 8
│       ├── develop_project.png      # Слайд 5
│       ├── criteria.png             # Слайд 6
│       ├── github.png               # Слайд 7
│       └── graduation.png           # Слайд 9
└── .claude/
    └── skills/
        └── zerocoder-presentation.md  # Этот файл
```

---

## Запуск презентации

Презентация — статический HTML, сервер не нужен. Открывается напрямую в браузере.

```powershell
# Открыть в браузере (Windows)
Start-Process "C:\Users\kvzay\OneDrive\Документы\fINAL\presentation\index.html"
```

Или просто дважды кликнуть на `presentation/index.html`.

**Управление слайдами:**
- `→` / `←` — следующий / предыдущий
- Точки внизу — переход к слайду
- Кнопки по бокам экрана
- Свайп на мобильном

---

## Архитектура index.html

Все 10 слайдов — секции `<section class="slide" id="slide-N">` внутри `.slides-track`.

**Общая структура слайда:**
```html
<section class="slide" id="slide-N">
  <div class="slide-bg" style="background-image:url('images/PHOTO.png')"></div>
  <!-- декор: .orb, .glow-line -->
  <div class="slide-content wide"> <!-- или single-col -->
    <div>
      <div class="slide-num anim-up">0N — Название</div>
      <h2 class="slide-title anim-up delay-1">Заголовок <span>акцент</span></h2>
      <!-- контент -->
    </div>
    <div class="slide-img anim-left delay-2">
      <img src="images/PHOTO.png" alt="...">
    </div>
  </div>
  <div class="wm-badge">zerocoder</div>
</section>
```

**Варианты layout:**
- `slide-content wide` — 55% текст + 40% картинка (большинство слайдов)
- `slide-content single-col` — один центрированный столбец (слайды 1 и 10)

**Цвета (CSS-переменные):**
- `var(--purple)` — `#9b7fe8` — акцент заголовков, кнопок
- `var(--green)` — `#4dee8d` — ключевые слова, буллеты, счётчик
- `var(--light)` — `#f0ecfc` — основной текст
- `var(--dark)` — `#2d2d2d` — фон

**Компоненты для контента слайда:**

| Класс | Назначение |
|---|---|
| `.bullet-list` | Маркированный список с зелёными точками |
| `.steps-grid` | Сетка 2×N с карточками шагов |
| `.step-card` | Карточка шага (`step-num`, `step-title`, `step-desc`) |
| `.criteria-grid` | Сетка критериев с галочками |
| `.criteria-item` | Элемент критерия |
| `.speech-list` | Нумерованный список выступления |
| `.insight-box` | Выделенный блок с ключевой мыслью |
| `.code-block` | Блок с кодом или промптом (monospace, зелёный) |
| `.time-badge` | Бейдж с иконкой ⏱ |
| `.countdown-bar` / `.c-box` | Счётчик недель на hero-слайде |

**Анимации (добавляются к элементам):**
- `anim-up` — появление снизу вверх
- `anim-left` — появление справа налево (для картинки)
- `anim-right` — появление слева направо
- `anim-scale` — появление с масштабированием
- `delay-1` … `delay-6` — задержка анимации (0.1s … 0.8s)

---

## Как вносить изменения

### Изменить текст на слайде
Найди нужный слайд по `id="slide-N"` или по номеру `slide-num`, отредактируй текст внутри тегов.

### Добавить новый слайд
1. Скопируй блок существующего слайда нужного типа
2. Поменяй `id="slide-N"` на следующий номер
3. Обнови счётчик в HTML: `<div class="slide-counter">... / 11</div>` (строка ~615)
4. Обнови переменную `total` в JS, если она задана хардкодом (проверь — сейчас `querySelectorAll('.slide').length` считает автоматически)

### Изменить фоновое изображение слайда
```html
<!-- было -->
<div class="slide-bg" style="background-image:url('images/hero.png')"></div>
<!-- стало -->
<div class="slide-bg" style="background-image:url('images/new_image.png')"></div>
```
Не забудь положить новый файл в `presentation/images/`.

### Добавить новый компонент
Используй готовые CSS-классы из таблицы выше. Для нового уникального блока добавь стили в секцию `<style>` в `<head>`.

---

## Проверка ошибок

**Что проверять перед публикацией:**

1. **Все изображения на месте** — каждый `url('images/...')` должен иметь файл в `presentation/images/`
2. **Счётчик слайдов** — число в `.slide-counter` совпадает с реальным количеством `<section class="slide">`
3. **Анимации** — все интерактивные элементы имеют класс `anim-up` или аналог
4. **Валидность HTML** — открыть в браузере, проверить консоль DevTools (F12) на ошибки
5. **Мобильная версия** — проверить в DevTools (Toggle Device Toolbar) при ширине 375px

**Быстрая проверка изображений:**
```powershell
# Список всех файлов изображений
ls "C:\Users\kvzay\OneDrive\Документы\fINAL\presentation\images"
```

---

## Обновление README.md

README описывает проект для GitHub. Структура:
- Название и одна строка описания
- Что это такое (тип проекта, стек)
- Какую задачу решает
- Для кого (пользователь + контекст использования)
- Таблица содержания слайдов
- Инструкция по запуску
- Управление слайдами
- Структура файлов

При изменении количества слайдов или их тем — обновить таблицу содержания в README.

---

## Публикация на GitHub

Репозиторий уже создан и подключён к remote origin.

```powershell
# 1. Проверить статус изменений
git status

# 2. Добавить изменённые файлы
git add presentation/index.html
# или для всех изменений
git add .

# 3. Создать коммит
git commit -m "краткое описание изменений"

# 4. Отправить на GitHub
git push

# 5. Проверить результат
gh repo view --web
```

**Полезные команды gh:**
```powershell
# Открыть репозиторий в браузере
gh repo view --web

# Посмотреть статус репозитория
gh repo view

# Создать релиз
gh release create v1.0 --title "Версия 1.0" --notes "Описание релиза"
```

---

## GitHub Pages (публикация как сайт)

Презентацию можно опубликовать как сайт через GitHub Pages — тогда у неё будет публичная ссылка.

```powershell
# Включить GitHub Pages из ветки master, папка /presentation
gh api repos/ZerocoderForstudents/zerocoder-final-lesson/pages `
  --method POST `
  --field source='{"branch":"master","path":"/presentation"}'
```

После включения сайт будет доступен по адресу:  
`https://zerocodeforstudents.github.io/zerocoder-final-lesson/`

---

## Частые задачи

| Задача | Что делать |
|---|---|
| Показать презентацию | `Start-Process presentation/index.html` |
| Добавить слайд | Скопировать блок slide, поменять id и контент |
| Изменить цвет акцента | Поменять `--purple` или `--green` в `:root` |
| Добавить слайд с кодом | Использовать `.code-block` внутри `.slide-content` |
| Опубликовать изменения | `git add . && git commit -m "..." && git push` |
| Открыть репо на GitHub | `gh repo view --web` |
| Сделать публичный URL | Включить GitHub Pages (см. выше) |
