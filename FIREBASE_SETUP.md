# Инструкция по настройке Firebase

## Шаг 1: Установка зависимостей

Установите Firebase SDK:
```bash
npm install firebase
```

## Шаг 2: Создание проекта в Firebase

1. Перейдите на [Firebase Console](https://console.firebase.google.com/)
2. Создайте новый проект или выберите существующий
3. В настройках проекта перейдите в раздел "Ваши приложения"
4. Добавьте веб-приложение (иконка `</>`)
5. Скопируйте конфигурационные данные

## Шаг 3: Настройка переменных окружения

1. Создайте файл `.env.local` в корне проекта
2. Скопируйте содержимое из `.env.example` в `.env.local`
3. Заполните значениями из консоли Firebase:

```env
VITE_FIREBASE_API_KEY=ваш-api-key
VITE_FIREBASE_AUTH_DOMAIN=ваш-project-id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=ваш-project-id
VITE_FIREBASE_STORAGE_BUCKET=ваш-project-id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=ваш-messaging-sender-id
VITE_FIREBASE_APP_ID=ваш-app-id
```

## Шаг 4: Настройка Firebase Authentication

1. В консоли Firebase перейдите в раздел **Authentication**
2. Нажмите "Начать"
3. Включите метод входа **Email/Password**
4. Сохраните изменения

## Шаг 5: Настройка Firestore Database

1. В консоли Firebase перейдите в раздел **Firestore Database**
2. Создайте базу данных (режим "Начать в тестовом режиме" для разработки)
3. Выберите регион (например, `europe-west`)

## Шаг 6: Создание коллекции cities

1. В Firestore Database создайте коллекцию с именем `cities`
2. Добавьте документы со следующей структурой:

### Пример структуры документа:
- **ID документа**: `msc` (или любой другой уникальный ID)
- **Поля**:
  - `name` (string): "Москва"
  - `id` (string): "msc" (опционально)

### Рекомендуемые города для добавления:
```
msc: { name: "Москва", id: "msc" }
spb: { name: "Санкт-Петербург", id: "spb" }
ovb: { name: "Новосибирск", id: "ovb" }
svx: { name: "Екатеринбург", id: "svx" }
other: { name: "Другой", id: "other" }
```

**Примечание**: Приложение также поддерживает поле `value` вместо `name`, или использует ID документа как fallback.

## Шаг 7: Настройка правил безопасности Firestore

В разделе **Правила** Firestore Database установите следующие правила для разработки:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Правила для коллекции cities (только чтение)
    match /cities/{document=**} {
      allow read: if true;
    }
    
    // Правила для коллекции users (запись только для аутентифицированных пользователей)
    match /users/{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

**Важно**: Эти правила подходят только для разработки. Для продакшена настройте более строгие правила безопасности.

## Шаг 8: Запуск приложения

```bash
npm run dev
```

## Структура данных в Firestore

### Коллекция `cities`
Каждый документ содержит:
- `name` (string) - название города
- `id` (string, опционально) - идентификатор города

### Коллекция `users`
Каждый документ содержит:
- `uid` (string) - уникальный идентификатор пользователя из Firebase Auth
- `email` (string) - email пользователя
- `firstname` (string) - имя
- `lastname` (string) - фамилия
- `country` (string) - страна/регион
- `city` (string) - город
- `phone` (string) - телефон
- `comments` (string, опционально) - дополнительная информация (только если заполнено)
- `createdAt` (string) - дата и время создания в формате ISO

## Обработка ошибок

Приложение обрабатывает следующие ошибки Firebase:
- `auth/email-already-in-use` - "Пользователь с таким e-mail уже существует"
- `auth/weak-password` - "Пароль слишком слабый"
- `auth/invalid-email` - "Неверный формат email"
- Другие ошибки отображаются с общим сообщением

## Проверка работы

1. Убедитесь, что города загружаются в выпадающий список
2. Заполните форму регистрации
3. Проверьте, что пользователь создается в Firebase Authentication
4. Проверьте, что данные сохраняются в коллекцию `users` в Firestore
