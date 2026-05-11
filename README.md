# Бие даалт 14 — API Testing: Postman + Newman

**F.CSM311 Программ хангамжийн бүтээлт**  
**API:** JSONPlaceholder (`https://jsonplaceholder.typicode.com`)  
**Auth:** Шаардахгүй

---

## Бүтэц

```
bie-daalt-14/
├── .github/workflows/api-tests.yml   # CI/CD
├── partA/
│   ├── SETUP.md                      # API тайлбар
│   └── screenshot.png                # Эхний request
├── postman/
│   ├── collection.json               # 8 request, 3 folder
│   ├── env.dev.json                  # Dev орчин
│   └── env.ci.json                   # CI орчин
├── reports/
│   └── api.html                      # Newman HTML тайлан
├── README.md
└── REFLECTION.md
```

---

## Ажиллуулах заавар

### 1. Newman суулгах

```bash
npm install -g newman newman-reporter-htmlextra
```

### 2. Local-д ажиллуулах

```bash
newman run postman/collection.json -e postman/env.dev.json
```

### 3. HTML report үүсгэх

```bash
newman run postman/collection.json \
  -e postman/env.dev.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export reports/api.html
```

---

## Collection-ийн агуулга

| Folder | Request | Зорилго |
|--------|---------|---------|
| Posts | Happy GET — List all posts | Бүх posts жагсаах, chain эхлүүлэх |
| Posts | GET by ID — Single post | Chain: postId ашиглан нэг post авах |
| Posts | POST — Create new post | Pre-request script + 201 шалгах |
| Posts | PUT — Update post | 200 ба өөрчлөлт шалгах |
| Posts | DELETE — Delete post | 200 шалгах |
| Users | Happy GET — List all users | email regex шалгах |
| Users | GET by ID — Single user | Schema + type шалгах |
| Errors | 404 Not Found | Negative test — байхгүй нөөц |

**Нийт тест assertion: 25+**  
**Assertion төрөл: status, latency, schema, type, business rule, header**

---

## CI/CD

GitHub Actions дээр `push` болон `pull_request` үед автоматаар ажиллана.  
Actions tab → `API Tests` workflow → тайланг artifact болгон татаж авна.
