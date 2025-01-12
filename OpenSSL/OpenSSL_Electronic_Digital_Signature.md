Формирование самоподписного сертификата с помощью Openssl: `openssl req -x509 -noenc -newkey rsa:2048 -keyout newkey.pem -out newcert.pem -days 365` (вместо `-noenc` можно использовать устаревшее `-nodes`)
- cert.pem — сертификат, содержащий открытый ключ и информацию о том, кому выдан;
- cert.key — закрытый ключ, предназначенный для создания подписей или расшифровки.

Верификация самоподписанного сертификата: `openssl verify -CAfile newcert.pem newcert.pem`

Вывод информации о самоподписанном сертификате: `openssl x509 -in newcert.pem -text -noout` (В выводе и открытый ключ, и подпись сертификата и информация о
владельце и сроки использования)

Создание электронной подписи файла: `openssl dgst -sha256 -sign newkey.pem -out doksignature.bin dok.txt`

Получение открытого ключа из сертификата: `openssl x509 -pubkey -noout -in newcert.pem > newpublic_key.pem`

Проверка подписи с помощью открытого ключа: `openssl dgst -sha256 -verify newpublic_key.pem -signature doksignature.bin dok.txt`