Два варианта: `openssl dgst -hash_command` и `openssl hash_command`

`time openssl dgst -md5 dok.txt`

`time openssl dgst -sha1 dok.txt`

Зашифровать файл: `openssl enc -des-cbc -in dok.txt -out dok_encrypted.des -k 12345678 -provider legacy -provider default`
Создание хэша (аутентификатора) зашифрованного файла:
`openssl dgst -md5 dok_encrypted.des > dok_encrypted_hash.md5`
`openssl dgst -md5 dok_encrypted.des`

Сгенерировать 2048 битный закрытый ключ rsa: `openssl genrsa -out keys.rsa 2048`
Сгенерировать 2048 битный закрытый ключ rsa: `openssl rsa -in keys.rsa -out pubkey.rsa -pubout`
Вывести информацию о закрытом ключе: `openssl rsa -in keys.rsa -text -noout`

Подписать хэш-сумму sha512 от файла dok.txt закрытым ключом keys.rsa и записать подпись в файл dok.sig: `openssl sha512 -sign keys.rsa -out dok.sig dok.txt`

Проверить подпись хэш-суммы sha512 из файла dok.sig для файла dok.txt по алгоритму RSA: `openssl sha512 -verify pubkey.rsa -signature dok.sig dok.txt`

Снять защиту ключа парольной фразой: `openssl rsa -in keys.rsa -passin pass:1234 -out keys_nopass.rsa`