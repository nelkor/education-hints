# Полезные советы тем, кто учится

## Автоматическое форматирование и проверка качества кода

Если мы пишем JavaScript-код в .js-файлах, есть возможность применить
автоматическое форматирование и поиск простых ошибок.

Для этого локально установим необходимые инструменты при помощи `npm`.
Вызываем команду в консоли в директории проекта:

```shell
npm i -D eslint globals prettier @eslint/js eslint-plugin-prettier @stylistic/eslint-plugin-js eslint-plugin-perfectionist
```

Создадим файл конфигурации для Prettier. Этот файл должен находиться в корне
репозитория и называться `.prettierrc`. Копируем в него:

```json
{
  "semi": false,
  "singleQuote": true,
  "arrowParens": "avoid",
  "quoteProps": "consistent"
}
```

Любители точки с запятой могут удалить строку `"semi": false`.

Создадим файл конфигурации для ESLint. Имя файла — `eslint.config.js`.
Копируем в него:

```javascript
import perfectionistPlugin from 'eslint-plugin-perfectionist'
import stylisticJsPlugin from '@stylistic/eslint-plugin-js'
import prettierPlugin from 'eslint-plugin-prettier'
import jsPlugin from '@eslint/js'
import globals from 'globals'

export default [
  jsPlugin.configs.recommended,
  perfectionistPlugin.configs['recommended-line-length'],
  {
    rules: {
      'stylistic/padding-line-between-statements': [
        'error',
        // В будущем заменить правила для 'import' на eslint-plugin-import.
        // Сейчас eslint-plugin-import не работает на ESLint 9.
        { blankLine: 'always', prev: 'import', next: '*' },
        { blankLine: 'any', prev: 'import', next: 'import' },
        { blankLine: 'always', next: 'return', prev: '*' },
        { blankLine: 'always', prev: 'block-like', next: '*' },
        { blankLine: 'always', next: 'block-like', prev: '*' },
        { blankLine: 'always', prev: 'export', next: '*' },
        { blankLine: 'always', next: 'export', prev: '*' },
        { prev: 'singleline-const', blankLine: 'always', next: '*' },
        { prev: 'singleline-let', blankLine: 'always', next: '*' },
        { next: 'singleline-const', blankLine: 'always', prev: '*' },
        { next: 'singleline-let', blankLine: 'always', prev: '*' },
        { prev: 'singleline-let', next: 'singleline-let', blankLine: 'any' },
        {
          prev: 'singleline-const',
          next: 'singleline-const',
          blankLine: 'any',
        },
      ],
      'no-promise-executor-return': 'error',
      'array-callback-return': 'error',
      'no-unused-expressions': 'error',
      'prefer-arrow-callback': 'error',
      'prefer-destructuring': 'error',
      'prettier/prettier': 'error',
      'consistent-return': 'error',
      'arrow-body-style': 'error',
      'object-shorthand': 'error',
      'no-return-assign': 'error',
      'no-await-in-loop': 'error',
      'no-throw-literal': 'error',
      'no-extend-native': 'error',
      'no-return-await': 'error',
      'prefer-template': 'error',
      'no-else-return': 'error',
      'accessor-pairs': 'error',
      'no-lone-blocks': 'error',
      'require-await': 'error',
      'prefer-const': 'error',
      'dot-notation': 'error',
      'no-multi-str': 'error',
      'camelcase': 'error',
      'no-proto': 'error',
      'curly': 'error',
    },
    languageOptions: {
      globals: {
        ...globals.browser,
        ...globals.es2025,
      },
    },
    plugins: {
      stylistic: stylisticJsPlugin,
      prettier: prettierPlugin,
    },
    files: ['**/*.js'],
  },
  {
    ignores: ['dist', 'node_modules'],
  },
]
```

Переходим в файл `package.json`. Если там нет ключа `scripts`, то добавляем его.
Если там нет записи `"type": "module"`, то добавляем её. Помимо того, что там
уже есть добавляем следующее:

```json
{
  "type": "module",
  "scripts": {
    "lint": "eslint",
    "lint:fix": "eslint --fix"
  }
}
```

В нашем распоряжении появляются две команды: `npm run lint` и `npm run lint:fix`.
Первая команда проверяет код и, в случае обнаружения недостатков, сообщает о них
в консоль. Вторая команда также проверяет код и, в случае обнаружения недостатков,
пытается самостоятельно их починить. Если починить не получилось, то уже тогда
сообщает в консоль.
