# 1 file
## series
series.dtype - тип данных внутри series
series.nunique() - количество уникальных значений
series.index - получить объект Index() с массивом индексов
series.values - получить объект Array() с массивом значений
series.name = 'new series name' -  присвоение / изменение имени
series.index.name = 'new series index name' -  присвоение / изменение имени индекса
series.head(n) - возвращает первые n строк.
series.tail(n) - возвращает последние n строк
`series.loc[[25,33]]`  - поиск ячеек по двум id
`series.iloc[[5,-5]]` - получаем ячейки с позициями 5 и -5
del series['Maggie'] - удаляет ячейку (строчку) по индексу
series.pop('Sector') - удаляет ячейку и возвращает её
series.where((numbers > 0) & (numbers < 1), other = -1) - возрващает строки удовлетворяющие условию, а значение оставшихся заменяем на -1
(series >= 0).all() - возвращает true, если все строки данной серии удовл условию, иначе возвращает false
(series >= 0).any() - возвращает true, если хотя бы одна строка данной серии удовл условию, иначе возвращает false
## dataframe
df.columns = ['col_1', 'col_2'] - задаем имена столбцов после создания датафрейма
len(df) - количество строк
df.size - количество ячеек
df.shape - (количество строк, количество столбцов)
df.dtypes - тип данных каждой колонки
df.nunique() - количество уникальных значений в каждой колонке
df.index - получить объект Index() с массивом индексов
df.values - получить объект Array() с массивом значений (по сути строк)
df.columns - получить объект Index() с массивом названий колонок
df_copy = df.rename(columns = {'Book Value': 'BookValue'}) - присвоение / изменение имени столбца
df_copy.rename(columns = {'Book Value': 'BookValue'}, inplace=True) - переименовывает столбец на месте
df.head(n=5) - возвращает первые n строк.
df.tail(n) - возвращает последние n строк
df['Sector'].head() - извлекает столбец 'Sector'
type(df['Sector'])  - извлекает тип столбца 'Sector' (pandas.core.series.Series)
`df[['Price', 'Book Value']].head()` - извлекает столбцы 'Price' и 'Book Value'
`type(df[['Price', 'Book Value']])` - извлекает тип датафрейма из двух столбцов 'Sector' и 'Book Value' (pandas.core.frame.DataFrame)
df.Price.head() - атрибутивный доступ к столбцу по имени (не рекомендуется т. к. df.Book Value - выдаст ошибку)
`df.loc['MMM']`  - поиск строки по id, возвращает series (если строка 1)
`df.loc[['MMM', 'MSFT']]`  - поиск строк по двум id, возвращает dataFrame (если строк > 1)
series
`df.iloc[[0, 2]]` - получаем строки, имеющие позиции 0 и 2
df.index.get_loc('MMM') - получаем позиции меток MMM в индексе
df.at['MMM', 'Price'] - доступ к одному значению для пары меток строки/столбца
df.iat[0, 1] - Доступ к одному значению для пары строк/столбцов по целочисленной позиции
`df.loc[['ABT', 'ZTS']][['Sector', 'Price']]` - получение строк с метками индекса ABT и ZTS для столбцов Sector и Price
`df.iloc[[1,499],[0,1]]` - отбор строк и столбцов по номеру позиций
df.T - транспонирование
df.reindex(index=['MMM', 'ABBV', 'NEW VALUE']) - переиндексация
`df.reindex(columns=['Price', 'Book Value', 'NewCol'])` - переиндексация столбцов
`df.reindex(columns=['Price', 'Book Value', 'NewCol'], fill_value=0)` - `fill_value=0` - заполняет отсутствующие значения константами вместо _NaN_
`df.sample(frac=5, replace=True, random_state=777)` - Возвращает случайную выборку элементов из оси объекта; `frac` - доля элементов оси для возврата; `replace` - разрешить или запретить выборку одной и той же строки более одного раза; `random_state` - начальное число для генератора случайных чисел
`pd.options.display.max_rows` and `pd.options.display.max_columns` устанавливают максимальное количество отображаемых строк и столбцов, когда кадр красиво напечатан

`df_new = df.iloc[[1,2,3,4]]` - копируем строки
`df_new = df[1:5]` - ссылаемся на них

del df['MMM'] - столбец по названию или строку по индексу
df.pop('Sector') - удаляет стобец и возвращает его
df.drop(['Sector'], axis = 1) - возвращает dataframe без столбца 'Sector'
df.iloc[:5].copy() - возвращает копию первых 5 строк
`df[(sp500['Price'] < 10) & (sp500.Price > 6)]` - извлекаем лишь те строки, в которых значение Price < 10 и > 6
`df[(sp500['Price'] < 10) & (sp500.Price > 6)][['Price', 'Sector']]` - извлекаем лишь те строки (только колонки 'Price' и 'Sector'), в которых значение Price < 10 и > 6
df.isin(_values_) - Содержится ли каждый элемент в df в values (`df.Sector.isin(['Information Technology', 'Financials'])`)

`df.query("Sector=='Health Care' & Price >= 100")[['Price', 'Sector']]` - запрашивает столбцы DataFrame с помощью логического выражения
df['RoundedPrice'] = df_copy.Price.round() - добавить новый столбец 'RoundedPrice'
df_copy.insert(1, 'RoundedPrice', df_copy.Price.round()) - вставляем столбец RoundedPrice в качестве третьего столбца датафрейма
`df.assign(kwargs)` - добавляет новые столбцы в df

df.sort_index() - сортирует df по индуксу(столбцу или строке)
df.sort_values(by='name_value') - сортирует df по значению столбца; ascending=False - в обратном порядке

df.nsmallest(n, 'Price') и df.nlargest(n, 'Price')  - возвращает первые n наименьших/наибольших значений в столбце 'Price'



Parameter **axis**: {0 or ‘index’, 1 or ‘columns’}, default 0 - Ось, по которой выполняется сортировка. Значение 0 определяет строки, а 1 — столбцы.

Правильный способ интерпретировать параметр оси - это то, какую ось вы суммируете «по» (или «поперек»), а не «направление», в котором вычисляется сумма. Указание оси = 0 вычисляет сумму по строкам, давая вам итог по каждому столбцу; оси = 1 вычисляет сумму по столбцам, давая вам итог для каждой строки.


#############################################################

# 2 file

DataFrame.xs(_key_, _axis=0_, _level=None_, _drop_level=True_)
Parameters:
- **key**: метка или кортеж метки (Метка, содержащаяся в индексе или частично в MultiIndex.)
- **axis**{0 or ‘index’, 1 or ‘columns’}, default 0 (Ось для извлечения поперечного сечения.(По вертикали или по горизонтали))
- **level**object, defaults to first n levels (n=1 or len(key)) (Если ключ частично содержится в MultiIndex, укажите, какие уровни используются. Уровни можно указывать по метке или позиции.)
- **drop_level**bool, default True (Если False, возвращает объект с теми же уровнями, что и изначально)

Правильный способ интерпретировать параметр оси - это то, какую ось вы суммируете «по» (или «поперек»), а не «направление», в котором вычисляется сумма. Указание оси = 0 вычисляет сумму по строкам, давая вам итог по каждому столбцу; оси = 1 вычисляет сумму по столбцам, давая вам итог для каждой строки.

# 3 file

DataFrame.isnull() - Обнаружение пропущенных значений.

DataFrame.isnull — это псевдоним для DataFrame.isna.

Возвращает логический объект того же размера, указывающий, являются ли значения NA. Значения NA, такие как None или numpy.NaN, сопоставляются со значениями True. Все остальное сопоставляется со значениями False. Такие символы, как пустые строки '' или numpy.inf, не считаются значениями NA (если вы не установите pandas.options.mode.use_inf_as_na = True).

DataFrame.apply(_func_, _axis=0_, _raw=False_, _result_type=None_, _args=()_, _**kwargs_) - Примените функцию вдоль оси DataFrame.

Объекты, передаваемые функции, представляют собой объекты Series, индекс которых является либо индексом DataFrame (ось = 0), либо столбцами DataFrame (ось = 1). По умолчанию (result_type=None) окончательный тип возвращаемого значения выводится из типа возвращаемого значения примененной функции. В противном случае это зависит от аргумента result_type.

DataFrame.applymap(_func_, _na_action=None_, _**kwargs_) - Примените функцию к Dataframe поэлементно.

Этот метод применяет функцию, которая принимает и возвращает скаляр для каждого элемента DataFrame.

# 4 file

**set_index**
DataFrame.set_index(_keys_, _*_, _drop=True_, _append=False_, _inplace=False_, _verify_integrity=False_)
**inplace**bool, default False - Нужно ли изменять DataFrame, а не создавать новый.

**Процентиль**
Процентиль — это значение, которое заданная случайная величина не превышает с фиксированной вероятностью, заданной в процентах.

**std()**
Std - стандартное отклонение

**top**
top является наиболее распространенным значением. freq — это частота наиболее распространенного значения.

**normalize: bool, default False**
Если True, то возвращаемый объект будет содержать относительные частоты уникальных значений.

**numpy.random.randn**
Размеры возвращаемого массива должны быть неотрицательными. Если аргумент не указан, возвращается одно число с плавающей запятой Python.

**sub()**
Метод sub() вычитает каждое значение в DataFrame с указанным значением. Указанное значение должно быть объектом, который можно вычесть из значений в DataFrame.

**Ковариация**
В теории вероятностей и математической статистике — **мера совместной вариативности (линейной зависимости) двух случайных величин**. Если обе величины демонстрируют однонаправленное изменение, то ковариация положительная, а если разнонаправленное — отрицательная.

Корреляция
Корреляция (от лат. correlatio), корреляционная зависимость — взаимозависимость двух или нескольких случайных величин. Суть ее заключается в том, что **при изменении значения одной переменной происходит закономерное изменение (уменьшению или увеличению) другой(-их) переменной(-ых)**.

**normalize**
Нормализация вектора означает, что его векторная величина равна 1 как единичный вектор.

**pct_change()**
Вы можете использовать **функцию pct_change()** для расчета процентного изменения между значениями в pandas.

**center: bool, default False**
Если False, установите метки окна как правый край индекса окна.  
Если True, установите метки окна как центр индекса окна.


Теперь, когда мы обсудили, как использовать qcut , можем показать, чем он отличается от cut . Основное различие заключается в том, что **qcut будет вычислять размер каждого интервала, чтобы гарантировать, что распределение данных в интервалах одинаково**.

# 5 file

DataFrame.pct_change(_periods=1_, _fill_method='pad'_, _limit=None_, _freq=None_, _**kwargs_)

Процентное изменение между текущим и предыдущим элементом.  
  
По умолчанию вычисляет процентное изменение по сравнению с непосредственно предыдущей строкой. Это полезно при сравнении процента изменения во временном ряду элементов.

Parameters: **periods**: int, default 1
Периоды сдвига для формирования процентного изменения.

**fill_method**: str, default ‘pad’
Как обрабатывать NA перед вычислением процентных изменений.

**limit**: int, default None
Количество последовательных NA, которые нужно заполнить перед остановкой.

**freq**: DateOffset, timedelta, or str, optional
Увеличение для использования из API временных рядов (например, «M» или BDay()).

****kwargs**
Дополнительные аргументы ключевого слова передаются в DataFrame.shift или Series.shift.

Returns: **chg**: Series or DataFrame
Тот же тип, что и вызывающий объект.

## 5 file

**on**label : or list

- Column or index level names to join on. These must be found in both DataFrames. If on is None and not merging on indexes then this defaults to the intersection of the columns in both DataFrames.

**left_on**label or list, or array-like

- Column or index level names to join on in the left DataFrame. Can also be an array or list of arrays of the length of the left DataFrame. These arrays are treated as if they are columns.

**right_on**label or list, or array-like

- Column or index level names to join on in the right DataFrame. Can also be an array or list of arrays of the length of the right DataFrame. These arrays are treated as if they are columns.

**left_index**bool, default False

- Use the index from the left DataFrame as the join key(s). If it is a MultiIndex, the number of keys in the other DataFrame (either the index or a number of columns) must match the number of levels.

**right_index**bool, default False

- Use the index from the right DataFrame as the join key. Same caveats as left_index.

**sort**bool, default False

- Sort the join keys lexicographically in the result DataFrame. If False, the order of the join keys depends on the join type (how keyword).

**suffixes**list-like, default is (“_x”, “_y”)

- A length-2 sequence where each element is optionally a string indicating the suffix to add to overlapping column names in left and right respectively. Pass a value of None instead of a string to indicate that the column name from left or right should be left as-is, with no suffix. At least one of the values must not be None.

**copy**bool, default True

- If False, avoid copy if possible.

**indicator**bool or str, default False

- If True, adds a column to the output DataFrame called “_merge” with information on the source of each row. The column can be given a different name by providing a string argument. The column will have a Categorical type with the value of “left_only” for observations whose merge key only appears in the left DataFrame, “right_only” for observations whose merge key only appears in the right DataFrame, and “both” if the observation’s merge key is found in both DataFrames.

**validate**str, optional

- If specified, checks if merge is of specified type.

-   “one_to_one” or “1:1”: check if merge keys are unique in both left and right datasets.
    
-   “one_to_many” or “1:m”: check if merge keys are unique in left dataset.
    
-   “many_to_one” or “m:1”: check if merge keys are unique in right dataset.
    
-   “many_to_many” or “m:m”: allowed, but does not result in checks.


# 6 file

GroupBy.nth - Возьмите n-ю строку из каждой группы, если n является int, в противном случае подмножество строк.

Вы можете использовать pandas DataFrame.groupby().count() для группировки столбцов и вычисления количества или агрегированного размера, при этом вычисляется количество строк для каждой комбинации групп.