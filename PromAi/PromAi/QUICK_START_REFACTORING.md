# ⚡ Быстрый старт: Рефакторинг test1.html

## 🎯 Цель
Переделать `test1.html` под стандарты PromAi с MODULE ID, FUNC ID и data-атрибутами.

## 📁 Что уже создано

```
PromAi/
├── test1.html                      ← Оригинальный файл
├── test1_refactored.html           ← Демо с MODULE/FUNC ID
├── REFACTORING_GUIDE.md            ← Полное руководство
├── project_registry.json           ← Реестр модулей
├── dependencies_map.json           ← Граф зависимостей
├── todo.json                       ← Задачи
└── changelog.md                    ← История изменений
```

## 🚀 3 шага для начала

### Шаг 1: Изучи структуру (5 мин)

**Открой файлы:**
1. `test1_refactored.html` - посмотри как добавлены ID
2. `project_registry.json` - посмотри список модулей
3. `dependencies_map.json` - посмотри зависимости

**Ключевые изменения:**
```javascript
// БЫЛО:
const dataManager = {
    getNodeByPath(path) {
        // код
    }
};

// СТАЛО:
const dataManager = {
    moduleId: 'MODULE_DataManager_VER_1.0',  // ← Добавлено
    version: '1.0',                           // ← Добавлено
    
    getNodeByPath(path) {
        const funcId = 'FUNC_getNodeByPath_001';  // ← Добавлено
        console.log(`[${this.moduleId}][${funcId}] Executing...`);  // ← Добавлено
        // код
    }
};
```

### Шаг 2: Выбери подход (1 мин)

**Вариант A: Постепенный рефакторинг** (рекомендуется)
- Работай с `test1.html` напрямую
- Добавляй ID модуль за модулем
- Тестируй после каждого модуля

**Вариант B: Полная переделка**
- Создай новую структуру с нуля
- Перенеси функциональность по модулям
- Разбей на отдельные файлы сразу

### Шаг 3: Начни с TASK_001 (30 мин)

**Открой `todo.json` и начни:**

```json
{
  "id": "TASK_001",
  "title": "Рефакторинг test1.html - добавить MODULE ID",
  "status": "inProgress"
}
```

**Что делать:**

1. **Добавь MODULE ID в каждый модуль:**
```javascript
const dataManager = {
    moduleId: 'MODULE_DataManager_VER_1.0',
    version: '1.0',
    // ... остальной код
};
```

2. **Добавь FUNC ID в каждую функцию:**
```javascript
getNodeByPath(path) {
    const funcId = 'FUNC_getNodeByPath_001';
    console.log(`[${this.moduleId}][${funcId}] Getting node by path:`, path);
    
    try {
        // ... код функции
    } catch (error) {
        console.error(`[${this.moduleId}][${funcId}] Error:`, error);
    }
}
```

3. **Проверь в консоли:**
```
[MODULE_DataManager_VER_1.0][FUNC_getNodeByPath_001] Getting node by path: [0, 1]
[MODULE_UIManager_VER_1.0][FUNC_showModal_001] Showing modal: categoryModal
```

## 📋 Чеклист для TASK_001

- [ ] MODULE_DataManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] FUNC_getNodeByPath_001
  - [ ] FUNC_saveToLocalStorage_002
  - [ ] FUNC_addCategory_003
  - [ ] FUNC_addNode_004
  - [ ] FUNC_addTechnology_005

- [ ] MODULE_UIManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] FUNC_showModal_001
  - [ ] FUNC_hideModals_002
  - [ ] FUNC_showNotification_003

- [ ] MODULE_AccordionManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] Добавлен dependencies
  - [ ] FUNC_renderAccordion_001
  - [ ] FUNC_buildAccordion_002
  - [ ] FUNC_toggleItem_003

- [ ] MODULE_ChecklistManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] Добавлен dependencies
  - [ ] FUNC_manageChecklist_001
  - [ ] FUNC_addChecklistItem_002
  - [ ] FUNC_toggleChecklistItem_003

- [ ] MODULE_AuthManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] Добавлен dependencies
  - [ ] FUNC_testAuth_001
  - [ ] FUNC_saveAuth_002
  - [ ] FUNC_loadFromGitHub_003
  - [ ] FUNC_saveToGitHub_004
  - [ ] FUNC_autoSaveToGitHub_005

- [ ] MODULE_KnowledgeManager_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] Добавлен dependencies
  - [ ] FUNC_openKnowledgeBase_001
  - [ ] FUNC_saveContent_002
  - [ ] FUNC_renderMedia_003
  - [ ] FUNC_performSearch_004

- [ ] MODULE_KnowledgeEditor_VER_1.0
  - [ ] Добавлен moduleId
  - [ ] Добавлен version
  - [ ] Добавлен dependencies
  - [ ] FUNC_formatText_001
  - [ ] FUNC_insertTable_002
  - [ ] FUNC_insertLink_003
  - [ ] FUNC_insertImage_004

## 🎨 Шаблон для копирования

```javascript
// === MODULE_{Name}_VER_1.0 ===
const {moduleName}Manager = {
    moduleId: 'MODULE_{Name}_VER_1.0',
    version: '1.0',
    dependencies: [], // или ['MODULE_Other_VER_1.0']
    
    // FUNC_{functionName}_{number}
    {functionName}() {
        const funcId = 'FUNC_{functionName}_{number}';
        console.log(`[${this.moduleId}][${funcId}] Executing...`);
        
        try {
            // Код функции
            
        } catch (error) {
            console.error(`[${this.moduleId}][${funcId}] Error:`, error);
            throw error;
        }
    }
};
```

## ✅ Проверка результата

После завершения TASK_001:

1. **Откройте консоль браузера**
2. **Выполните действия в приложении**
3. **Проверьте логи:**

```
✅ Правильно:
[MODULE_DataManager_VER_1.0][FUNC_addCategory_003] Adding category: JavaScript

❌ Неправильно:
Adding category: JavaScript  (нет MODULE/FUNC ID)
```

## 📊 Прогресс

```
TASK_001: Добавить MODULE ID        [▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░] 50%
TASK_002: Добавить data-атрибуты    [░░░░░░░░░░░░░░░░░░░░]  0%
TASK_003: Разделить на файлы        [░░░░░░░░░░░░░░░░░░░░]  0%
TASK_004: Создать тесты             [░░░░░░░░░░░░░░░░░░░░]  0%
TASK_005: Оптимизировать            [░░░░░░░░░░░░░░░░░░░░]  0%
```

## 🔄 После TASK_001

Когда закончишь TASK_001:

1. **Обнови todo.json:**
```json
{
  "id": "TASK_001",
  "status": "done",
  "actualHours": 2.5
}
```

2. **Обнови changelog.md:**
```markdown
## [1.1.0] - 2024-01-15
### Added
- MODULE ID для всех 7 модулей
- FUNC ID для всех 25+ функций
- Логирование с идентификаторами
```

3. **Сделай коммит:**
```bash
git add test1.html todo.json changelog.md
git commit -m "feat(ALL_MODULES): add MODULE_ID and FUNC_ID to all modules"
```

4. **Переходи к TASK_002** (data-атрибуты)

## 💡 Советы

1. **Используй поиск и замену** для быстрого добавления ID
2. **Тестируй после каждого модуля** - не делай всё сразу
3. **Смотри в test1_refactored.html** если не уверен
4. **Логируй всё** - это поможет при отладке
5. **Следуй naming convention** - MODULE_{Name}_VER_{version}

## 🆘 Если что-то не работает

1. **Проверь консоль** - там будут ошибки с MODULE/FUNC ID
2. **Сравни с test1_refactored.html** - там рабочий пример
3. **Проверь project_registry.json** - там список всех ID
4. **Используй @code-review** в Amazon Q для проверки

## 📚 Дополнительно

- **Полное руководство:** `REFACTORING_GUIDE.md`
- **Стандарты проекта:** `.amazonq/rules/project-standards.md`
- **Примеры промптов:** `FULL_PROMPTS_1-12.md`

---

**Готов начать? Открой `test1.html` и добавь первый MODULE ID! 🚀**
