

- **Структура:** Документы, ключ-значение, графы, колоночные.
    
- **Примеры:** MongoDB (документы), Redis (ключ-значение), Cassandra (колоночная).
    
- **Запросы:** Гибкие, но без стандартного SQL (часто API-запросы).
    
- **Масштабирование:** Горизонтальное ([[Шардирование]]).
    
- **Транзакции:** Обычно [[BASE]], иногда ограниченный [[ACID]].

##### Очень удобно и гибко горизонтально  масштабируются за счет [[Шардирование]], на основе ***Consistent Hashing***


##### Основным минусом является - отсутствие ***консистентности данных***, это значит что после записи в БД, результат сразу возвращается пользователю, а процесс копирования с master-node на slave-node работает асинхронно, данные когда-то станут консистентными, но когда - не известно.
