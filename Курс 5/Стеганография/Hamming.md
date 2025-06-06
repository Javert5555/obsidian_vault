# Подробное пошаговое объяснение реализации стеганографии с кодом Хемминга

## 1. Инициализация класса и матрицы Хемминга

```python
def __init__(self, input_file):
    self.input_file = input_file
    self.header_size = 54  # Фиксированный размер заголовка BMP
    
    # Проверочная матрица H для (15,11)-кода Хемминга
    self.H = np.array([
        [1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1],
        [0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1],
        [0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1]
    ])
```

**Что происходит**:
1. Создается проверочная матрица H размером 4×15
2. Каждый столбец матрицы - двоичное представление чисел от 1 до 15
3. Матрица будет использоваться для кодирования и декодирования

## 2. Процесс внедрения сообщения (embed)

### Шаг 1: Подготовка сообщения
```python
if isinstance(message, str):
    message = message.encode('utf-8')

message_length = len(message)
message = struct.pack('>I', message_length) + message
```

**Что происходит**:
1. Сообщение преобразуется в байты (если это строка)
2. Длина сообщения упаковывается в 4 байта и добавляется в начало

### Шаг 2: Преобразование в биты
```python
bits = []
for byte in message:
    bits.extend([(byte >> i) & 1 for i in range(7, -1, -1)])
```

**Что происходит**:
1. Каждый байт разбивается на 8 бит
2. Биты добавляются в общий список

### Шаг 3: Группировка битов
```python
bit_groups = [bits[i:i+4] for i in range(0, len(bits), 4)]
```

**Что происходит**:
1. Биты группируются по 4 (поскольку мы кодируем 4 бита в 15 пикселей)

### Шаг 4: Проверка размера изображения
```python
total_pixels_needed = len(bit_groups) * 15
available_pixels = self.image_size
if total_pixels_needed > available_pixels:
    raise ValueError(...)
```

**Что происходит**:
1. Проверяется, хватит ли места в изображении для сообщения

### Шаг 5: Внедрение групп битов
```python
for group in bit_groups:
    # Получаем LSB 15 пикселей
    C = [self.data[data_index + i] & 1 for i in range(15)]
    
    # Вычисляем синдром
    m_vec = np.array(group).reshape(4, 1)
    C_vec = np.array(C).reshape(15, 1)
    s = (self.H @ C_vec) % 2
    s = s ^ m_vec
    
    # Вычисляем позицию для изменения
    i = int(s[3]*8 + s[2]*4 + s[1]*2 + s[0])
    
    # Модифицируем i-й бит
    if 0 < i <= 15:
        self.data[data_index + i - 1] ^= 1
    
    data_index += 15
```

**Что происходит**:
1. Для каждой группы из 4 бит:
   - Берем LSB 15 последовательных пикселей
   - Вычисляем синдром (H·C mod 2)
   - Вычисляем позицию для изменения: i = s₃·8 + s₂·4 + s₁·2 + s₀
   - Инвертируем i-й бит (если i ≠ 0)
## Общий контекст

- **bit_groups** — список групп по 4 бита (это фрагменты скрываемого сообщения, разбитые по 4 бита).
    
- **self.data** — массив байтов изображения (например, пиксели BMP-файла).
    
- **self.H** — проверочная матрица (H) кода Хэмминга (размера 4×15).
    
- **data_index** — текущий индекс в массиве self.data, указывающий, с какого пикселя работать.
    

Код внедряет 4 бита информации в 15 пикселей, изменяя максимум 1 бит в каждом блоке из 15 пикселей. Это делается для минимизации искажений и повышения стойкости к обнаружению.

---

## Подробный разбор

`for group in bit_groups:` - Проходим по каждой группе из 4 бит (часть сообщения для внедрения).

`if len(group) < 4:     group += [0] * (4 - len(group))` - Если последняя группа содержит меньше 4 бит (например, сообщение не делится нацело), дополняем её нулями до 4 бит.

`C = [] for i in range(15):     if data_index + i < len(self.data):        C.append(self.data[data_index + i] & 1)    else:        C.append(0)`
- Собираем массив **C** из 15 младших бит (LSB) следующих 15 пикселей.
- Если пикселей не хватает (конец файла), дополняем нулями.

`m_vec = np.array(group, dtype=np.uint8).reshape(4, 1) C_vec = np.array(C, dtype=np.uint8).reshape(15, 1)`
- **m_vec** — вектор сообщения (4×1).
- **C_vec** — вектор LSB текущих 15 пикселей (15×1).

`s = (self.H @ C_vec) % 2`
- Умножаем проверочную матрицу Хэмминга **H** на вектор LSB (C_vec).
- Получаем синдром **s** (4×1), который показывает, есть ли ошибка (или, в нашем случае, как "исправить" LSB, чтобы внедрить сообщение).

`s = s ^ m_vec`
- Выполняем побитовый XOR между синдромом и вектором сообщения.
- Получаем новый синдром, который указывает, какой бит нужно изменить, чтобы внедрить нужные 4 бита сообщения.

`i = int(s[3] * 8 + s[2] * 4 + s[1] * 2 + s[0])`
- Преобразуем 4 бита синдрома в целое число (индекс позиции).
- Это номер бита (от 1 до 15), который нужно инвертировать в LSB, чтобы внедрить сообщение.

`if 0 < i <= 15 and (data_index + i - 1) < len(self.data):     self.data[data_index + i - 1] ^= 1`
- Если индекс i в допустимых пределах (от 1 до 15) и не выходит за границы массива данных:
    - Инвертируем соответствующий LSB (операция XOR с 1).
    - Таким образом, ровно один бит в группе из 15 пикселей меняется, чтобы внедрить 4 бита сообщения.

`data_index += 15` - Сдвигаем указатель на следующие 15 пикселей для следующей группы из 4 бит сообщения.

## Итоговая логика

- Каждая группа из 4 бит сообщения внедряется в 15 пикселей, изменяя максимум один LSB, используя код Хэмминга.
    
- Это позволяет минимизировать искажения, сделать внедрение менее заметным и повысить устойчивость к обнаружению.
    
- Такой подход эффективнее простого LSB-внедрения, где каждый бит сообщения меняет отдельный пиксель.
    

---

## Почему так делается?

- **Код Хэмминга** позволяет внедрять 4 бита в 15 пикселей, изменяя максимум 1 бит — это гораздо менее заметно, чем менять 4 бита в 4 пикселях.
- Такой способ часто используется в стеганографии для повышения скрытности и устойчивости к ошибкам.

## 3. Процесс извлечения сообщения (extract)

### Шаг 1: Чтение групп пикселей
```python
while data_index + 15 <= len(self.data):
    C = [self.data[data_index + i] & 1 for i in range(15)]
    
    # Вычисляем сообщение
    C_vec = np.array(C).reshape(15, 1)
    m = (self.H @ C_vec) % 2
    
    bits.extend(m.flatten().tolist()[:4])
    
    data_index += 15
```

**Что происходит**:
1. Для каждой группы из 15 пикселей:
   - Извлекаем LSB
   - Вычисляем сообщение: m = H·C mod 2
   - Добавляем 4 бита сообщения в общий список

### Шаг 2: Преобразование битов в сообщение
```python
# Извлекаем длину (первые 32 бита)
message_length = 0
for i in range(32):
    message_length = (message_length << 1) | bits[i]

# Извлекаем само сообщение
message_bits = bits[32:32 + message_length*8]
```

**Что происходит**:
1. Первые 32 бита преобразуются в длину сообщения
2. Затем извлекается само сообщение

## 4. Математическая основа

Код Хемминга позволяет:
1. Закодировать 4 бита информации в 15 бит (пикселей)
2. Обнаруживать и исправлять ошибки
3. В стеганографии мы используем его для минимального изменения пикселей

Формулы:
- При внедрении: i = H·C ⊕ m
- При извлечении: m = H·C mod 2

Где:
- H - проверочная матрица
- C - LSB пикселей
- m - биты сообщения
- i - позиция для изменения

## 5. Пример работы

**Внедрение**:
1. Есть сообщение: "A" (01000001)
2. Разбиваем на группы: [0100], [0001]
3. Для первой группы:
   - Берем 15 LSB пикселей: C
   - Вычисляем H·C mod 2
   - Находим i = H·C ⊕ m
   - Инвертируем i-й бит

**Извлечение**:
1. Для тех же 15 пикселей:
   - Вычисляем m = H·C mod 2
   - Получаем исходные 4 бита

Этот метод обеспечивает высокую скрытность, так как изменяет минимальное количество пикселей для кодирования информации.


Приведу пример исправления одиночной ошибки в коде Хэмминга (15,11) на основе проверочной матрицы HHH.

---

## 1. Проверочная матрица HHH для кода (15,11)

Матрица HHH имеет размер 4×15, где каждый столбец — это двоичное представление номера позиции (от 1 до 15):

H=(000000011111111000111100001111011001100110011101010101010101)H = \begin{pmatrix} 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 & 1 & 1 & 1 & 1 \\ 0 & 0 & 0 & 1 & 1 & 1 & 1 & 0 & 0 & 0 & 0 & 1 & 1 & 1 & 1 \\ 0 & 1 & 1 & 0 & 0 & 1 & 1 & 0 & 0 & 1 & 1 & 0 & 0 & 1 & 1 \\ 1 & 0 & 1 & 0 & 1 & 0 & 1 & 0 & 1 & 0 & 1 & 0 & 1 & 0 & 1 \\ \end{pmatrix}H=000100100011010001010110011110001001101010111100110111101111

Каждый столбец — это 4-битное двоичное число позиции (считая сверху вниз)[3](https://xity.narod.ru/coding/p07.pdf)[6](https://onti.polyus-nt.ru/mod/page/view.php?id=224).

---

## 2. Пример кодового слова

Пусть передано кодовое слово (15 бит), например:

c=1 0 1 1 0 0 1 1 0 0 1 1 0 0 1c = 1\,0\,1\,1\,0\,0\,1\,1\,0\,0\,1\,1\,0\,0\,1c=101100110011001

---

## 3. Ошибка при передаче

Пусть в процессе передачи произошла ошибка в 7-м бите, и он инвертировался:

r=1 0 1 1 0 0 0 1 0 0 1 1 0 0 1r = 1\,0\,1\,1\,0\,0\,\mathbf{0}\,1\,0\,0\,1\,1\,0\,0\,1r=101100010011001

---

## 4. Вычисление синдрома

Синдром SSS вычисляется как:

S=r×HTmod  2S = r \times H^T \mod 2S=r×HTmod2

То есть умножаем вектор принятого слова rrr на транспонированную проверочную матрицу HTH^THT.

В результате получаем 4-битный синдром, который совпадает со столбцом HHH, соответствующим позиции ошибки.

Для ошибки в 7-м бите синдром будет равен 7 в двоичном виде: 011101110111.

---

## 5. Определение позиции ошибки и исправление

- Синдром S=0111S = 0111S=0111 указывает на позицию 7 (число 7 в двоичной форме).
    
- Значит, ошибка в 7-м бите.
    
- Инвертируем 7-й бит обратно с 0 на 1.
    

---

## 6. Итог

Исправленное кодовое слово:

c=1 0 1 1 0 0 1 1 0 0 1 1 0 0 1c = 1\,0\,1\,1\,0\,0\,1\,1\,0\,0\,1\,1\,0\,0\,1c=101100110011001

---

## Краткое резюме

- Проверочная матрица HHH содержит двоичные номера позиций.
    
- Синдром — результат умножения принятого слова на HTH^THT.
    
- Синдром указывает позицию одиночной ошибки.
    
- Ошибка исправляется инверсией бита в этой позиции.
    

Так работает исправление ошибок в коде Хэмминга (15,11) на основе проверочной матрицы и синдрома[3](https://xity.narod.ru/coding/p07.pdf)[6](https://onti.polyus-nt.ru/mod/page/view.php?id=224).

1. [https://habr.com/ru/articles/511348/](https://habr.com/ru/articles/511348/)
2. [https://mmf.bsu.by/wp-content/uploads/2016/10/Line_code_code_seq.pdf](https://mmf.bsu.by/wp-content/uploads/2016/10/Line_code_code_seq.pdf)
3. [https://xity.narod.ru/coding/p07.pdf](https://xity.narod.ru/coding/p07.pdf)
4. [https://habr.com/ru/articles/140611/](https://habr.com/ru/articles/140611/)
5. [https://studfile.net/preview/16433164/page:3/](https://studfile.net/preview/16433164/page:3/)
6. [https://onti.polyus-nt.ru/mod/page/view.php?id=224](https://onti.polyus-nt.ru/mod/page/view.php?id=224)
7. [https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%B4_%D0%A5%D1%8D%D0%BC%D0%BC%D0%B8%D0%BD%D0%B3%D0%B0](https://ru.wikipedia.org/wiki/%D0%9A%D0%BE%D0%B4_%D0%A5%D1%8D%D0%BC%D0%BC%D0%B8%D0%BD%D0%B3%D0%B0)
8. [https://github.com/b0006/Hamming-code](https://github.com/b0006/Hamming-code)