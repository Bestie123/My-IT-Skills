# 🔄 Руководство по рефакторингу test1.html под стандарты PromAi

## 📋 Что было сделано

### 1. Созданы файлы состояния
- ✅ `project_registry.json` - реестр из 7 модулей
- ✅ `dependencies_map.json` - граф зависимостей
- ✅ `todo.json` - 5 задач для дальнейшей работы
- ✅ `changelog.md` - история изменений

### 2. Идентифицированы модули

| Модуль | ID | Функции | Зависимости |
|--------|----|---------| ------------|
| Data Manager | MODULE_DataManager_VER_1.0 | 5 функций | - |
| UI Manager | MODULE_UIManager_VER_1.0 | 3 функции | - |
| Accordion Manager | MODULE_AccordionManager_VER_1.0 | 3 функции | DataManager |
| Checklist Manager | MODULE_ChecklistManager_VER_1.0 | 3 функции | DataManager, UIManager |
| Auth Manager | MODULE_AuthManager_VER_1.0 | 5 функций | DataManager, UIManager |
| Knowledge Manager | MODULE_KnowledgeManager_VER_1.0 | 4 функции | DataManager, UIManager |
| Knowledge Editor | MODULE_KnowledgeEditor_VER_1.0 | 4 функции | KnowledgeManager |

### 3. Создан test1_refactored.html
Демонстрационная версия с:
- MODULE ID для каждого модуля
- FUNC ID для каждой функции
- data-атрибуты для Inspector
- Логирование с идентификаторами
- Четкая структура зависимостей

## 🎯 Следующие шаги

### Шаг 1: Полный рефакторинг (TASK_001)
```javascript
// Добавить MODULE ID во все модули
const dataManager = {
    moduleId: 'MODULE_DataManager_VER_1.0',
    version: '1.0',
    dependencies: [],
    
    // Каждая функция с FUNC ID
    getNodeByPath(path) {
        const funcId = 'FUNC_getNodeByPath_001';
        console.log(`[${this.moduleId}][${funcId}] Executing...`);
        // ... код
    }
};
```

### Шаг 2: Data-атрибуты (TASK_002)
```html
<!-- Для каждого DOM элемента -->
<div class="container" 
     data-module-id="MODULE_UIManager_VER_1.0" 
     data-component-id="COMP_MainContainer">
    
    <button onclick="dataManager.addCategory()" 
            data-function-id="FUNC_addCategory_003">
        Add Category
    </button>
</div>
```

### Шаг 3: Разделение на файлы (TASK_003)
```
src/
├── modules/
│   ├── DataManager.js          # MODULE_DataManager_VER_1.0
│   ├── UIManager.js            # MODULE_UIManager_VER_1.0
│   ├── AccordionManager.js     # MODULE_AccordionManager_VER_1.0
│   ├── ChecklistManager.js     # MODULE_ChecklistManager_VER_1.0
│   ├── AuthManager.js          # MODULE_AuthManager_VER_1.0
│   ├── KnowledgeManager.js     # MODULE_KnowledgeManager_VER_1.0
│   └── KnowledgeEditor.js      # MODULE_KnowledgeEditor_VER_1.0
├── index.html
└── main.js
```

### Шаг 4: Unit тесты (TASK_004)
```javascript
// tests/DataManager.test.js
describe('MODULE_DataManager_VER_1.0', () => {
    describe('FUNC_getNodeByPath_001', () => {
        it('should return node by valid path', () => {
            const path = [0, 1];
            const result = dataManager.getNodeByPath(path);
            expect(result).toBeDefined();
        });
        
        it('should return null for invalid path', () => {
            const path = [999];
            const result = dataManager.getNodeByPath(path);
            expect(result).toBeNull();
        });
    });
});
```

### Шаг 5: Оптимизация (TASK_005)
- Виртуализация для больших списков
- Debounce для поиска
- Мемоизация для расчета прогресса
- Lazy loading для медиа

## 🔍 Использование Inspector

После добавления data-атрибутов:

1. **Наведите на элемент** - увидите:
   - `data-module-id`: какой модуль владеет элементом
   - `data-component-id`: ID компонента
   - `data-function-id`: какая функция создала элемент

2. **Отладка**:
   ```javascript
   // Найти все элементы модуля
   document.querySelectorAll('[data-module-id="MODULE_DataManager_VER_1.0"]');
   
   // Найти элементы функции
   document.querySelectorAll('[data-function-id="FUNC_renderAccordion_001"]');
   ```

## 📊 Граф зависимостей

```
MODULE_DataManager_VER_1.0 (Core)
    ↑
    ├── MODULE_AccordionManager_VER_1.0
    ├── MODULE_ChecklistManager_VER_1.0
    ├── MODULE_AuthManager_VER_1.0
    └── MODULE_KnowledgeManager_VER_1.0
            ↑
            └── MODULE_KnowledgeEditor_VER_1.0

MODULE_UIManager_VER_1.0 (Core)
    ↑
    ├── MODULE_ChecklistManager_VER_1.0
    ├── MODULE_AuthManager_VER_1.0
    └── MODULE_KnowledgeManager_VER_1.0
```

## 🎨 Принципы SOLID

### Single Responsibility
- ✅ DataManager - только данные
- ✅ UIManager - только UI
- ✅ AccordionManager - только аккордеон

### Open/Closed
```javascript
// Расширяемо через наследование
class BaseManager {
    constructor(moduleId, version) {
        this.moduleId = moduleId;
        this.version = version;
    }
    
    log(funcId, message) {
        console.log(`[${this.moduleId}][${funcId}] ${message}`);
    }
}

class DataManager extends BaseManager {
    constructor() {
        super('MODULE_DataManager_VER_1.0', '1.0');
    }
}
```

### Liskov Substitution
```javascript
// Все менеджеры реализуют общий интерфейс
interface IManager {
    moduleId: string;
    version: string;
    init(): void;
    destroy(): void;
}
```

### Interface Segregation
```javascript
// Разделяем интерфейсы
interface IDataProvider {
    getData(): any;
    saveData(data: any): void;
}

interface IRenderer {
    render(): void;
    update(): void;
}
```

### Dependency Inversion
```javascript
// Зависим от абстракций, а не конкретных реализаций
class AccordionManager {
    constructor(dataProvider, uiManager) {
        this.dataProvider = dataProvider; // Интерфейс, а не конкретный класс
        this.uiManager = uiManager;
    }
}
```

## 🧪 Тестирование

### Минимальное покрытие: 80%

```bash
# Запуск тестов
npm test

# Покрытие
npm run test:coverage

# Ожидаемый результат:
# Statements   : 80% ( 120/150 )
# Branches     : 80% ( 40/50 )
# Functions    : 80% ( 24/30 )
# Lines        : 80% ( 115/144 )
```

## 📝 Git Commit Format

```bash
# Новая функциональность
git commit -m "feat(MODULE_DataManager): add FUNC_exportToJSON_006"

# Исправление бага
git commit -m "fix(MODULE_AccordionManager): fix FUNC_toggleItem_003 state bug"

# Рефакторинг
git commit -m "refactor(MODULE_UIManager): extract FUNC_createNotification_004"

# Тесты
git commit -m "test(MODULE_DataManager): add tests for FUNC_getNodeByPath_001"

# Документация
git commit -m "docs(MODULE_KnowledgeManager): update API documentation"
```

## 🚀 Быстрый старт

### 1. Сравните файлы
```bash
# Оригинал
test1.html

# Рефакторенная версия (демо)
test1_refactored.html
```

### 2. Изучите структуру
```bash
# Файлы состояния
project_registry.json      # Реестр модулей
dependencies_map.json      # Граф зависимостей
todo.json                  # Задачи
changelog.md               # История
```

### 3. Начните рефакторинг
```bash
# Создайте ветку
git checkout -b refactor/promai-standards

# Работайте по задачам из todo.json
# TASK_001 → TASK_002 → TASK_003 → ...

# Коммитьте по стандартам
git commit -m "feat(MODULE_DataManager): add MODULE_ID and FUNC_IDs"
```

## 📚 Дополнительные ресурсы

- `START_HERE.md` - главная документация PromAi
- `FULL_PROMPTS_1-12.md` - детальные промпты
- `.amazonq/rules/project-standards.md` - автоматические правила

## ✅ Чеклист рефакторинга

- [ ] Добавлены MODULE ID во все модули
- [ ] Добавлены FUNC ID во все функции
- [ ] Добавлены data-атрибуты в DOM
- [ ] Добавлено логирование с ID
- [ ] Созданы отдельные файлы модулей
- [ ] Написаны unit тесты (80%+ покрытие)
- [ ] Обновлен project_registry.json
- [ ] Обновлен dependencies_map.json
- [ ] Обновлен changelog.md
- [ ] Проверены все зависимости
- [ ] Оптимизирована производительность
- [ ] Добавлена документация API
- [ ] Проведен код-ревью

## 🎯 Результат

После полного рефакторинга вы получите:
- ✅ Модульную архитектуру с четкими границами
- ✅ Трассируемость через MODULE/FUNC ID
- ✅ Отладку через Inspector с data-атрибутами
- ✅ Автоматическое управление состоянием
- ✅ Высокое качество кода (SOLID, DRY, тесты)
- ✅ Простоту поддержки и расширения

---

**Следующий шаг:** Откройте `todo.json` и начните с TASK_001
