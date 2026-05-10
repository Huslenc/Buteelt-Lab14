# А хэсэг — Setup

## Сонгосон API: JSONPlaceholder

### Тайлбар
JSONPlaceholder нь REST API-г туршиж үзэх зориулалттай үнэгүй, нээлттэй хуурамч API юм.
Prototype болон тест бичихэд тохиромжтой бэлэн endpoint-уудыг хангадаг.

- **Сайт:** https://jsonplaceholder.typicode.com
- **Документаци:** https://jsonplaceholder.typicode.com/guide/

### Нөөцүүд (Resources)
- `/posts` — 100 нийтлэл (id, userId, title, body)
- `/users` — 10 хэрэглэгч (id, name, email, address, ...)
- `/todos` — 200 даалгавар (id, userId, title, completed)
- `/comments` — 500 сэтгэгдэл

### Auth
**Auth шаардахгүй.** Бүх endpoint-д API key болон token хэрэггүй.

> Тэмдэглэл: POST/PUT/DELETE үйлдлүүд бодитоор өгөгдөл өөрчлөхгүй ч 200/201 status-тай хариу буцаадаг.

### Base URL
```
https://jsonplaceholder.typicode.com
```

### Rate Limit
Тодорхой rate limit байхгүй. Нийтийн үйлчилгээ тул хэт олон хүсэлт явуулахаас зайлсхийх нь зүйтэй.

### Environment хувьсагчид
| Хувьсагч | Утга |
|----------|------|
| `baseUrl` | `https://jsonplaceholder.typicode.com` |
