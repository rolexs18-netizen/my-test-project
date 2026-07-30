## Тест-кейс 1: Обновление рейтинга при повторной отправке

**Цель:** Проверить, что повторный POST-запрос от того же пользователя к тому же товару обновляет существующий рейтинг, а не создает новый.

**Предусловия:**
- Пользователь alice авторизован (token: tok_alice).
- Товар с prod_id=1 существует.
- В таблице ratings уже есть запись от alice для prod_id=1 с rating=4 (создана предыдущим запросом).

**Шаги:**
1. Отправить POST-запрос на /addrating с телом:
   { "user_token": "tok_alice", "prod_id": 1, "rating": 5, "review": "Отлично!" }
2. Проверить статус-код ответа.
3. Выполнить SQL-запрос для проверки количества записей:
   SELECT COUNT(*) FROM ratings JOIN users ON ratings.user_id = users.user_id WHERE username = 'alice' AND prod_id = 1;
4. Выполнить SQL-запрос для проверки обновлённых данных:
   SELECT rating, review FROM ratings JOIN users ON ratings.user_id = users.user_id WHERE username = 'alice' AND prod_id = 1;

**Ожидаемый результат:**
- Статус-код: 200 OK.
- SQL (COUNT): 1 (только одна запись).
- SQL (данные): rating = 5, review = "Отлично!".