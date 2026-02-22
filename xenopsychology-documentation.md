# Ксенопсихология — Техническая документация веб-сайта

## Обзор реализации

Полнофункциональный прототип веб-сайта для курса "Ксенопсихология" — единый HTML файл с встроенным React, готовый к деплою.

### Что реализовано

#### 1. Макет главной страницы
- **Hero section** с анимированной сетевой диаграммой (20 узлов, пульсирующая анимация)
- Двойной CTA (Call-to-Action): "Исследовать программу" + "Методология"
- Адаптивная типографика (clamp для масштабирования)
- Grain texture overlay для аналоговой эстетики

#### 2. Интерактивная навигация
- Фиксированный header с backdrop-blur эффектом
- Плавная прокрутка к секциям (якорные ссылки)
- Модальные окна для детальной информации по каждому занятию
- Интерактивный граф структуры курса

#### 3. Копирайт для ключевых разделов

**Структура контента:**
- **Hero:** "Исследование психического как изменяющегося и множественного поля"
- **Ключевые принципы:** 3 карточки с основными тезисами
- **Программа курса:** 6 детальных блоков занятий
- **Методология:** двухколоночная структура с текстом и списком
- **Footer:** контактная информация и ссылки на ресурсы

**Tone of voice:**
- Академический, но доступный
- Использование точных терминов без упрощения
- Интрига через открытые вопросы, а не декларации

#### 4. Система диаграмм

##### Сетевая диаграмма (Hero)
- Генеративная: 20 узлов со случайными координатами
- Пульсирующая анимация с разными задержками
- Низкая opacity (0.15) для фонового эффекта

##### Граф взаимосвязей (Graph Section)
- 6 узлов-занятий с уникальными координатами
- Соединительные линии между последовательными занятиями
- Hover-эффекты: scale(1.1) + box-shadow glow
- Цветовая кодировка по зонам:
  - Психическое: #d4a574 (золотистый)
  - Режимы: #c97d5d (терракота)
  - Биологическое: #6b9d7c (зеленый)
  - ИИ: #5d82c9 (синий)
  - Немыслимое: #b45dc9 (фиолетовый)
  - Сборка: #8d5dc9 (лавандовый)

---

## Техническая архитектура

### Стек
- **React 18** (production build через CDN)
- **Babel Standalone** для JSX трансформации
- **Pure CSS** (без фреймворков)
- **Google Fonts:** IBM Plex Mono, Playfair Display, Work Sans

### Структура кода

```
index.html
├── <head>
│   ├── Google Fonts (3 семейства)
│   └── <style> (полный CSS, ~600 строк)
├── <body>
│   ├── .grain (texture overlay)
│   ├── #root (React mount point)
│   └── <script type="text/babel">
│       ├── sessionsData (6 занятий)
│       ├── principlesData (3 принципа)
│       ├── NetworkDiagram (компонент сети)
│       ├── SessionModal (компонент модального окна)
│       ├── SessionGraph (компонент графа)
│       └── App (главный компонент)
```

### CSS-переменные (легко кастомизировать)

```css
:root {
    --bg-primary: #0a0a0a;
    --bg-secondary: #121212;
    --text-primary: #e8e8e8;
    --text-secondary: #a0a0a0;
    
    --accent-psyche: #d4a574;
    --accent-regime: #c97d5d;
    --accent-bio: #6b9d7c;
    --accent-ai: #5d82c9;
    --accent-unthinkable: #b45dc9;
    --accent-assembly: #8d5dc9;
}
```

---

## Визуальный язык

### Типографика
- **Display (заголовки):** Playfair Display 900 — драматичность, академичность
- **Body:** Work Sans 300-600 — современная читабельность
- **Mono (акценты):** IBM Plex Mono — техническая точность

### Анимации
- **fadeInUp:** плавное появление контента (0.8s ease-out)
- **pulse:** пульсация узлов сети (2s infinite)
- Staggered delays (.delay-1, .delay-2, etc.)

### Эффекты
- **backdrop-filter: blur(20px)** — стеклянный header
- **box-shadow glow** на hover узлов графа
- **Grain texture** — SVG noise filter
- **Gradient borders** на hover карточек

---

## Кастомизация

### Изменить цветовую схему

**Для светлой темы:**
```css
:root {
    --bg-primary: #ffffff;
    --bg-secondary: #f5f5f5;
    --text-primary: #1a1a1a;
    --text-secondary: #666666;
}
```

### Добавить новое занятие

Добавить объект в `sessionsData`:
```javascript
{
    id: 7,
    number: 'Занятие 07',
    title: 'Название',
    subtitle: 'Подзаголовок',
    description: 'Описание...',
    themes: ['Тема 1', 'Тема 2'],
    color: '#hexcolor',
    instrumentalPart: 'Инструментальная часть...',
    x: 600,  // координата X в графе
    y: 300   // координата Y в графе
}
```

### Изменить позиции узлов в графе

Отредактировать `x` и `y` координаты в `sessionsData`:
```javascript
{
    x: 200,  // горизонталь (0-1200px)
    y: 100   // вертикаль (0-600px)
}
```

### Добавить анимацию перехода страниц

Установить React Router:
```javascript
// Обернуть компоненты в <AnimatePresence> из framer-motion
<AnimatePresence mode="wait">
    <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
    >
        {content}
    </motion.div>
</AnimatePresence>
```

---

## Оптимизация для продакшена

### 1. Разделить на файлы

**Структура:**
```
/src
  /components
    NetworkDiagram.jsx
    SessionModal.jsx
    SessionGraph.jsx
  /data
    sessions.js
    principles.js
  /styles
    global.css
    variables.css
  App.jsx
  index.js
```

### 2. Сборка через Vite

```bash
npm create vite@latest xenopsychology -- --template react
cd xenopsychology
npm install
# Копировать компоненты
npm run build
```

### 3. Оптимизация изображений

- Добавить WebP версии для фоновых текстур
- Lazy loading для модальных окон
- Использовать `<picture>` для адаптивных изображений

### 4. Деплой

**Рекомендации:**
- **Vercel:** `vercel --prod` (автоматический деплой из Git)
- **Netlify:** Drag & Drop build папки
- **Google Cloud Storage:** Статический хостинг
- **Firebase Hosting:** `firebase deploy`

---

## Интеграции

### Google Analytics

Добавить перед `</head>`:
```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-XXXXXXXXXX');
</script>
```

### Форма регистрации

Интегрировать с backend:
```javascript
function RegistrationForm() {
    const [email, setEmail] = useState('');
    
    const handleSubmit = async (e) => {
        e.preventDefault();
        await fetch('/api/register', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email })
        });
    };
    
    return <form onSubmit={handleSubmit}>...</form>;
}
```

### CMS интеграция (Strapi, Contentful)

Заменить `sessionsData` на API call:
```javascript
useEffect(() => {
    fetch('https://api.yourbackend.com/sessions')
        .then(res => res.json())
        .then(data => setSessions(data));
}, []);
```

---

## Адаптивность

### Breakpoints

- **Desktop:** > 1200px (оптимальное отображение)
- **Tablet:** 768px - 1199px (двухколоночные секции → одна колонка)
- **Mobile:** < 768px (скрытая навигация, упрощенный граф)

### Mobile-first улучшения

```css
@media (max-width: 768px) {
    .graph-node {
        width: 80px;
        height: 80px;
        font-size: 0.7rem;
    }
    
    .hero-title {
        font-size: 2rem;
    }
    
    .nav {
        display: none; /* Добавить hamburger menu */
    }
}
```

---

## Возможные расширения

### 1. Интерактивный промптинг-тренажер

Встроить iframe с Claude API для практической работы с ИИ прямо на сайте.

### 2. Визуализация прогресса

```javascript
function ProgressTracker() {
    const [completed, setCompleted] = useState([]);
    return (
        <div className="progress-ring">
            {sessionsData.map(session => (
                <div 
                    className={completed.includes(session.id) ? 'completed' : ''}
                >
                    {session.number}
                </div>
            ))}
        </div>
    );
}
```

### 3. Collaborative annotations

Интеграция с Hypothesis.is для коллективных аннотаций текстов курса.

### 4. Живая диаграмма исследовательских программ

Интерактивный граф связей между гипотезами студентов, обновляющийся в реальном времени через WebSocket.

---

## Лицензия и атрибуция

**Используемые ресурсы:**
- Google Fonts (Open Font License)
- React (MIT License)

**Авторство:**
Разработано для курса "Ксенопсихология" УРФУ, 2025

---

## Следующие шаги

1. **Тестирование:** Проверить на разных браузерах (Chrome, Firefox, Safari)
2. **Accessibility:** Добавить ARIA-labels, keyboard navigation
3. **SEO:** Meta tags, Open Graph, структурированные данные
4. **Performance:** Lighthouse audit, оптимизация времени загрузки
5. **Интеграция:** Подключить к реальному backend для регистраций

---

## Контакты для поддержки

Для технических вопросов по развертыванию и кастомизации:
- Документация React: https://react.dev
- CSS Grid Guide: https://css-tricks.com/snippets/css/complete-guide-grid/
- Vercel Deploy: https://vercel.com/docs

---

**Версия:** 1.0  
**Дата:** Январь 2025  
**Статус:** Готов к деплою