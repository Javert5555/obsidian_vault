Два варианта: `openssl enc -enc_command` и `openssl enc_command`

Генерация закрытого ключа с помощью rsa и шифрование этого ключа алгоритмом гост: `openssl genrsa -out privateRSA.pem -des 2048`

# Задания 3-7

## DES

3) `openssl rand 8 > mydeskey.key` - сгенерировать ключ
4) `openssl des -in ./text.txt -out ./des_text_encr.txt -k ./mydeskey.key` - зашифровать
6) `openssl des -d -in ./des_text_encr.txt -out ./des_text_decr.txt -k ./mydeskey_incorr.key` - расшифровать не правильно
7) `openssl des -d -in ./des_text_encr.txt -out ./des_text_decr.txt -k ./mydeskey.key` - расшифровать

## RSA

3) `openssl genrsa -out myrsaprivatekey.pem 1024` - сгенерировать закрытый ключ
4) 1.`openssl rsa -in myrsaprivatekey.pem -out myrsapublickey.key -pubout` - сгенерировать открытый ключ 
   2.`openssl rsautl -encrypt -in ./text.txt -out ./rsa_text_encr.txt -pubin -inkey myrsapublickey.key` - зашифровать файл
6) `openssl rsautl -decrypt -in ./rsa_text_encr.txt -out ./rsa_text_decr.txt -inkey myrsaprivatekey_incorr.pem` - расшифровать не правильно
7) `openssl rsautl -decrypt -in ./rsa_text_encr.txt -out ./rsa_text_decr.txt -inkey myrsaprivatekey.pem` - расшифровать

## DES3

3) `openssl rand 8 > mydes3key.key` - сгенерировать ключ
4) `openssl des3 -in ./text.txt -out ./des3_text_encr.txt -k ./mydes3key.key` - зашифровать
6) `openssl des3 -d -in ./des3_text_encr.txt -out ./des3_text_decr.txt -k ./mydes3key_incorr.key` - расшифровать не правильно
7) `openssl des3 -d -in ./des3_text_encr.txt -out ./des3_text_decr.txt -k ./mydes3key.key` - расшифровать

## DES-EDE

3) `openssl rand -base64 24 > mydesedekey.key` - сгенерировать ключ
4) `openssl des-ede -in ./text.txt -out ./desede_text_encr.txt -k ./mydesedekey.key` - зашифровать
6) `openssl des-ede -d -in ./desede_text_encr.txt -out ./desede_text_decr.txt -k ./mydesedekey_incorr.key` - расшифровать не правильно
7) `openssl des-ede -d -in ./desede_text_encr.txt -out ./desede_text_decr.txt -k ./mydesedekey.key` - расшифровать

## blowfish

3) `openssl rand -base64 32 > myblowfishkey.key` - сгенерировать ключ
4) `openssl bf-cbc -in ./text.txt -out ./blowfish_text_encr.txt -k ./myblowfishkey.key` - зашифровать
6) `openssl bf-cbc -d -in ./blowfish_text_encr.txt -out ./blowfish_text_decr.txt -k ./myblowfishkey_incorr.key` - расшифровать не правильно
7) `openssl bf-cbc -d -in ./blowfish_text_encr.txt -out ./blowfish_text_decr.txt -k ./myblowfishkey.key` - расшифровать

## blowfish CBC

3) `openssl rand -base64 16 > myblowfishcbckey.key` - сгенерировать ключ
openssl bf-cbc -e -in input_file.txt -out encrypted_file.txt -K blowfish_key.txt -base64
1) `openssl bf-cbc -in ./text.txt -out ./blowfishcbc_text_encr.bf64 -k ./myblowfishcbckey.key -base64` - зашифровать
2) `openssl bf-cbc -d -in ./blowfishcbc_text_encr.bf64 -out ./blowfishcbc_text_decr.txt -k ./myblowfishcbckey_incorr.key -base64` - расшифровать не правильно
3) `openssl bf-cbc -d -in ./blowfishcbc_text_encr.bf64 -out ./blowfishcbc_text_decr.txt -k ./myblowfishcbckey.key -base64` - расшифровать

без кодировки base64:
3) `openssl rand -base64 16 > myblowfishcbckey.key` - сгенерировать ключ
openssl bf-cbc -e -in input_file.txt -out encrypted_file.txt -K blowfish_key.txt -base64
1) `openssl bf-cbc -in ./text.txt -out ./blowfishcbc_text_encr.txt -k ./myblowfishcbckey.key` - зашифровать
2) `openssl bf-cbc -d -in ./blowfishcbc_text_encr.txt -out ./blowfishcbc_text_decr.txt -k ./myblowfishcbckey_incorr.key` - расшифровать не правильно
3) `openssl bf-cbc -d -in ./blowfishcbc_text_encr.txt -out ./blowfishcbc_text_decr.txt -k ./myblowfishcbckey.key` - расшифровать

## AES

3) `openssl rand 32 > myaes256key.key` - сгенерировать ключ
4) `openssl aes-256-cbc -in ./text.txt -out ./aes256_text_encr.txt -k ./myaes256key.key` - зашифровать
6) `openssl aes-256-cbc -d -in ./aes256_text_encr.txt -out ./aes256_text_decr.txt -k ./myaes256key_incorr.key` - расшифровать не правильно
7) `openssl aes-256-cbc -d -in ./aes256_text_encr.txt -out ./aes256_text_decr.txt -k ./myaes256key.key` - расшифровать