SELECT users.username, users.email
FROM users
LEFT JOIN orders ON users.user_id = orders.user_id
WHERE orders.order_id IS NULL