# Загрузка файлов с помощью Multer

## Запуск сервера

Открыть терминал в папке проекта и выполнить:

```bash
cd ~/Desktop/zad1
npm start
```

Ожидаемый результат:

```text
Сервер запущен на порту 3000
```

Сервер должен оставаться запущенным во время тестирования.

## Подготовка тестовых файлов

Открыть второй терминал и перейти в папку проекта:

```bash
cd ~/Desktop/zad1
```

Создать тестовые файлы:

```bash
echo "fake image content" > test.jpg
echo "test" > test.txt
```

## Загрузить аватар

```bash
curl -X POST http://localhost:3000/upload/avatar \
  -F "avatar=@test.jpg"
```

Ожидается статус `201` и сообщение:

```json
{
  "message": "Аватар загружен"
}
```

Файл сохраняется в папку `uploads/avatars`.

## Загрузить файл неправильного типа

```bash
curl -X POST http://localhost:3000/upload/avatar \
  -F "avatar=@test.txt"
```

Ожидается статус `400` и сообщение об ошибке:

```json
{
  "error": "Разрешены только изображения: JPEG, PNG, WebP, GIF"
}
```

## Загрузить несколько документов

```bash
curl -X POST http://localhost:3000/upload/documents \
  -F "documents=@test.txt" \
  -F "documents=@test.txt"
```

Ожидается статус `201` и сообщение о загрузке двух файлов.

Файлы сохраняются в папку `uploads/documents`.

## Загрузить профиль с файлом и текстовыми полями

```bash
curl -X POST http://localhost:3000/upload/profile \
  -F "name=Alice" \
  -F "bio=Backend developer" \
  -F "avatar=@test.jpg"
```

Ожидается статус `201` и ответ с полями `name`, `bio` и ссылкой на аватар.

## Загрузить изображение в память

```bash
curl -X POST http://localhost:3000/upload/image/process \
  -F "image=@test.jpg"
```

Ожидается статус `200` и сообщение:

```json
{
  "message": "Файл получен в памяти"
}
```

Файл обрабатывается через `req.file.buffer` и не сохраняется на диск.

## Удалить загруженный файл

Сначала посмотреть имя файла:

```bash
ls uploads/avatars
```

Затем подставить имя файла в команду:

```bash
curl -X DELETE http://localhost:3000/upload/avatars/ИМЯ_ФАЙЛА
```

Ожидаемый ответ:

```json
{
  "message": "Файл удалён"
}
```
