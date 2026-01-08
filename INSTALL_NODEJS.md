# Решение проблемы с npm

## Проблема
Ошибка: `npm : Имя "npm" не распознано как имя командлета`

Это означает, что Node.js (и npm) не установлен в системе или не добавлен в PATH.

## Решение: Установка Node.js

### Вариант 1: Установка через официальный сайт (Рекомендуется)

1. Перейдите на [nodejs.org](https://nodejs.org/)
2. Скачайте LTS версию (Long Term Support) - рекомендуется для стабильности
3. Запустите установщик и следуйте инструкциям
4. **Важно**: При установке убедитесь, что отмечена опция "Add to PATH"
5. После установки **перезапустите терминал/PowerShell**

### Вариант 2: Установка через Chocolatey (если установлен)

```powershell
choco install nodejs
```

### Вариант 3: Установка через winget (Windows 10/11)

```powershell
winget install OpenJS.NodeJS.LTS
```

## Проверка установки

После установки и перезапуска терминала выполните:

```powershell
node --version
npm --version
```

Должны отобразиться версии Node.js и npm.

## Установка зависимостей проекта

После установки Node.js выполните в корне проекта:

```powershell
npm install
```

Эта команда установит все зависимости, включая Firebase, который уже добавлен в `package.json`.

## Если Node.js уже установлен, но npm не работает

1. Проверьте, что Node.js установлен:
   ```powershell
   where.exe node
   ```

2. Если Node.js найден, но npm нет, возможно нужно:
   - Переустановить Node.js с опцией "Add to PATH"
   - Вручную добавить путь к npm в переменную окружения PATH
   - Обычно npm находится в: `C:\Program Files\nodejs\`

3. После изменений PATH **перезапустите терминал**

## Альтернатива: Использование nvm (Node Version Manager)

Если вы работаете с несколькими версиями Node.js:

1. Установите [nvm-windows](https://github.com/coreybutler/nvm-windows)
2. Установите Node.js через nvm:
   ```powershell
   nvm install lts
   nvm use lts
   ```

## После установки

1. Убедитесь, что `firebase` добавлен в `package.json` (уже сделано)
2. Выполните `npm install` в корне проекта
3. Следуйте инструкциям из `FIREBASE_SETUP.md` для настройки Firebase
