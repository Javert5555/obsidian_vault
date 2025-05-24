# Подробное построчное объяснение реализации LSB-Matching (LSB-M)

## 1. Инициализация класса и загрузка изображения

```python
import struct
import math
import random
```
Импортируем необходимые модули:
- `struct` - для работы с бинарными данными (упаковка/распаковка)
- `math` - для математических операций (округление и т.д.)
- `random` - для случайного выбора между +1 и -1 при внедрении

```python
class LSB_Matching_BMP:
    def __init__(self, input_file):
        self.input_file = input_file
        self.header_size = 54  # Размер заголовка BMP для 24-битных изображений
```
Создаем класс для работы с LSB-Matching. В конструкторе:
- `input_file` - путь к входному BMP-файлу
- `header_size` - фиксированный размер заголовка BMP (54 байта)

```python
        # Чтение файла
        with open(input_file, 'rb') as f:
            self.data = bytearray(f.read())
```
Открываем файл в бинарном режиме и читаем все данные в `bytearray` для модификации.

```python
        # Проверка формата файла
        if self.data[0] != ord('B') or self.data[1] != ord('M'):
            raise ValueError("Файл не является BMP-форматом")
```
Проверяем сигнатуру BMP (первые два байта должны быть 'B' и 'M').

```python
        self.image_size = len(self.data) - self.header_size
```
Вычисляем размер данных изображения (без заголовка).

## 2. Метод embed() - внедрение сообщения

```python
    def embed(self, message, output_file, rate=1.0):
        if isinstance(message, str):
            message = message.encode('utf-8')
```
Если сообщение - строка, преобразуем в байты (UTF-8).

```python
        # Добавляем длину сообщения (4 байта)
        message_length = len(message)
        message = struct.pack('>I', message_length) + message
```
Упаковываем длину сообщения (4 байта, big-endian) и добавляем перед сообщением.

```python
        # Преобразуем сообщение в биты
        bits = []
        for byte in message:
            bits.extend([(byte >> i) & 1 for i in range(7, -1, -1)])
```
Каждый байт сообщения разбиваем на 8 бит (от старшего к младшему).

```python
        total_bits = len(bits)
        available_bits = math.floor(self.image_size * rate * 3)
```
Вычисляем:
- `total_bits` - общее количество бит для внедрения
- `available_bits` - доступное количество бит (3 бита на пиксель RGB)

```python
        if total_bits > available_bits:
            raise ValueError("Сообщение слишком большое для изображения с заданным rate")
```
Проверяем, поместится ли сообщение в изображение.

```python
        # Внедряем биты с использованием LSB-Matching
        bit_index = 0
        step = math.ceil(1 / rate) if rate < 1.0 else 1
```
- `bit_index` - текущий индекс бита сообщения
- `step` - шаг между пикселями (рассчитывается из rate)

```python
        for i in range(self.header_size, len(self.data)):
            if (i - self.header_size) % step == 0 and bit_index < total_bits:
                current_bit = bits[bit_index]
                current_byte = self.data[i]
                current_lsb = current_byte & 1
```
Проходим по данным изображения и для каждого выбранного пикселя:
- `current_bit` - бит сообщения для внедрения
- `current_byte` - текущее значение байта изображения
- `current_lsb` - младший бит текущего байта

```python
                if current_bit == current_lsb:
                    # Бит уже совпадает - ничего не меняем
                    pass
```
Если бит сообщения совпадает с LSB - оставляем без изменений.

```python
                else:
                    if current_byte == 0:
                        # Особый случай: можно только увеличить на 1
                        self.data[i] = 1
                    elif current_byte == 255:
                        # Особый случай: можно только уменьшить на 1
                        self.data[i] = 254
```
Для крайних значений (0 и 255) используем LSB-Replacement.

```python
                    else:
                        # Случайный выбор между +1 и -1
                        self.data[i] = current_byte + random.choice([-1, 1])
```
Для остальных значений - случайный выбор между +1 и -1.

```python
                bit_index += 1
```
Переходим к следующему биту сообщения.

```python
        # Сохранение результата
        with open(output_file, 'wb') as f:
            f.write(self.data)
```
Сохраняем модифицированное изображение.

## 3. Метод extract() - извлечение сообщения

```python
    def extract(self, rate=1.0):
        bits = []
        step = math.ceil(1 / rate) if rate < 1.0 else 1
        max_bits = 32 * 8  # Максимум для длины сообщения
```
Инициализация:
- `bits` - список для извлеченных битов
- `step` - шаг между пикселями
- `max_bits` - максимальное количество бит для чтения (32 байта длины)

```python
        length_bits = []
        length_extracted = False
        message_length = 0
```
- `length_bits` - биты длины сообщения
- `length_extracted` - флаг извлечения длины
- `message_length` - длина сообщения

```python
        for i in range(self.header_size, len(self.data)):
            if (i - self.header_size) % step == 0:
                bit = self.data[i] & 1
                bits.append(bit)
                length_bits.append(bit)
```
Извлекаем LSB из выбранных пикселей.

```python
                if len(length_bits) == 32 and not length_extracted:
                    message_length = 0
                    for j in range(32):
                        message_length = (message_length << 1) | length_bits[j]
                    length_extracted = True
                    max_bits = 32 + message_length * 8
```
Когда собрали 32 бита длины - преобразуем в число и обновляем `max_bits`.

```python
                if len(bits) >= max_bits:
                    break
```
Прекращаем чтение после получения всех нужных битов.

```python
        if not length_extracted:
            return b''
```
Если не нашли длину - возвращаем пустую строку.

```python
        message_bits = bits[32:32 + message_length * 8]
```
Выделяем биты самого сообщения.

```python
        message = bytearray()
        for i in range(0, len(message_bits), 8):
            byte = 0
            for j in range(8):
                if i + j < len(message_bits):
                    byte = (byte << 1) | message_bits[i + j]
            message.append(byte)
```
Преобразуем биты в байты (по 8 бит).

```python
        return bytes(message)
```
Возвращаем извлеченное сообщение.

## 4. Пример использования

```python
if __name__ == "__main__":
    # Внедрение сообщения
    lsbm = LSB_Matching_BMP("PU24-test.bmp")
    secret_message = "Секретное сообщение методом LSB-Matching!"
    lsbm.embed(secret_message, "PU24-test_lsbm.bmp", rate=0.3)
    
    # Извлечение сообщения
    lsbm_stego = LSB_Matching_BMP("PU24-test_lsbm.bmp")
    extracted_message = lsbm_stego.extract(rate=0.3).decode('utf-8')
    print("Извлеченное сообщение:", extracted_message)
```
Пример показывает:
1. Внедрение сообщения с rate=0.3 (используется 30% пикселей)
2. Извлечение сообщения с тем же rate
3. Вывод извлеченного сообщения

Ключевое отличие LSB-Matching от LSB-Replacement - более сложный алгоритм внедрения, который случайным образом выбирает между +1 и -1, что делает обнаружение факта стеганографии более сложным.