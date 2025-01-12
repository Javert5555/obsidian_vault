## Regexp SQL
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[AEIOU]';` - выбрать все уникальные названия городов, которые начинаются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '[AEIOU]$';` - выбрать все уникальные названия городов, которые заканчиваются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY NOT REGEXP '[AEIOU]$';` - выбрать все уникальные названия городов, которые не заканчиваются на согласные
`SELECT DISTINCT CITY FROM STATION WHERE CITY REGEXP '^[AEIOU].*[AEIOU]$';` - выбрать все уникальные названия городов, которые заканчиваются и начинаются на согласные
