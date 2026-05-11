# REFLECTION — Бие даалт 14

## 1. Хамгийн үнэ цэнэтэй assertion аль байсан бэ?

Хамгийн үнэ цэнэтэй assertion нь **business rule шалгадаг тест** байсан — тухайлбал email
формат зөв эсэхийг regex-ээр шалгах тест (`pm.expect(d.email).to.match(/@/)`). 
Status code 200 буцаж байгаа нь зөв ажиллаж байна гэсэн үг биш. Өгөгдлийн агуулга 
тодорхой дүрмийг баримталж байгаа эсэхийг шалгах нь илүү чухал. Жишээлбэл, 
email-д `@` тэмдэг байхгүй бол front-end-д мэйл илгээхэд алдаа гарна — гэтэл 
status code шалгадаг тест энэ алдааг огт олохгүй.

## 2. Negative test-ийн жишээ

`GET /posts/999999` хүсэлт нь **404 Not Found** буцаахыг шалгадаг negative тест юм.
Энэ тест нь дараах алдааг олж чаддаг:

- API байхгүй нөөцийг `{}` буцааж байгаа ч 200 status буцаавал — нуугдмал алдаа
- Server exception гарч 500 буцаавал — алдааны зохицуулалт байхгүй байна гэсэн үг
- Timeout болвол — production-д хэрэглэгч удаан хүлээнэ

Зөвхөн "happy path" шалгах нь хангалтгүй. API-ийн contract нь зөв хариу өгөхөөс 
гадна алдааны нөхцөлд ч зөв хариу өгнө гэдгийг хамардаг.

## 3. Postman-д ажиллаж байсан тест Newman-д fail болсон уу?

Тийм. `Content-Type` header шалгадаг тест Newman-д анх fail болсон. Шалтгаан нь
Postman UI дээр header нэрийг `Content-Type` гэж бичсэн ч Newman дотоодын нэр нь 
`content-type` (жижиг үсэг) байсан. `pm.response.headers.get('Content-Type')` гэж
case-sensitive байгаа учир алдаа гарсан. `to.have.header()` assertion ашиглан засав.

Ерөнхийдөө Newman-д орвол fail болдог шалтгаанууд:
- Environment хувьсагч дутуу (`env.json` дамжуулаагүй)
- Харьцангуй URL ашигласан (`/posts` гэж hardcode хийсэн, `{{baseUrl}}` ашиглаагүй)
- OS-ийн SSL certificate ялгаа

## 4. Token / Secret зохицуулалт

JSONPlaceholder auth шаардахгүй тул token хэрэггүй болсон. Гэсэн хэдий ч 
environment-ийн зохистой соёлыг баримталсан:

- `env.dev.json` — бодит утгатай (baseUrl), repo-д commit хийсэн (secret байхгүй учир)
- `env.ci.json` — CI-д ашиглах тусгай файл, placeholder ашиглах боломжтой байдлаар бэлдсэн
- Token шаардагдах API сонгосон бол `REPLACE_THIS_WITH_REAL_TOKEN` placeholder
  ашиглан README-д тайлбарлах байсан
- GitHub Secrets-ийг env variable болгон workflow-д дамжуулна

**Дүрэм:** Real token, API key, password-г хэзээ ч git history-д үлдээхгүй.

## 5. API өөрчлөгдвөл хамгийн эмзэг хэсэг аль вэ?

**Chain хийсэн хүсэлтүүд** хамгийн эмзэг. Жишээлбэл:

1. `GET /posts` → `posts[0].id`-г `postId` болгон хадгалдаг
2. `GET /posts/{{postId}}` → энэ утгыг ашигладаг

Хэрэв API-ийн response-ийн field нэр өөрчлөгдвөл (`id` → `postId` болвол) бүх 
дараагийн chain хийсэн request-үүд нэгэн зэрэг fail болно.

**Эмзэг байдлыг бууруулах арга:**
- Schema validation тест нэмэх — field нэр өөрчлөгдмөгц мэдэгдэнэ
- Environment variable-ийн default утга тохируулах
- Collection-ийн эхэнд "sanity check" folder нэмэх — API амьд эсэхийг шалгах
- Versioned API endpoint ашиглах (`/v1/posts`) — breaking change-ийг удаашруулна
