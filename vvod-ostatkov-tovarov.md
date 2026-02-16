# Пример: Загрузка документа «Ввод остатков товаров»

Пошаговое руководство по загрузке данных из Excel-файла в документ **«Ввод остатков товаров»** (1С:ERP / 1С:Комплексная автоматизация) с помощью Loader1C.

---

## Содержание

1. [Подготовка Excel-файла](#1-подготовка-excel-файла)
2. [Загрузка файла в систему](#2-загрузка-файла-в-систему)
3. [Выбор типа объекта](#3-выбор-типа-объекта)
4. [Настройка маппинга реквизитов шапки](#4-настройка-маппинга-реквизитов-шапки)
5. [Настройка маппинга табличной части «Товары»](#5-настройка-маппинга-табличной-части-товары)
6. [Настройка параметров загрузки](#6-настройка-параметров-загрузки)
7. [Запуск загрузки и проверка результатов](#7-запуск-загрузки-и-проверка-результатов)
8. [JSON-конфигурация настроек](#8-json-конфигурация-настроек)

---

## 1. Подготовка Excel-файла

Подготовьте файл `Ввод остатков товаров.xlsx` со следующей структурой:

| N | Код | Артикул | Номенклатура | Назначение | Характеристика | Серия | Статус указания серий | Количество | Упаковка | Ед. изм. | Цена (RUB) | Сумма (RUB) | Ставка НДС | НДС (RUB) | Сумма с НДС | Сумма (регл.) | НДС (регл) | Детализация партий | Номер ГТД / РНПТ | Сумма (ПР) | Сумма (ВР) | Резерв (регл.) | Резерв (упр.) | Склад |
|---|-----|---------|--------------|------------|----------------|-------|-----------------------|------------|----------|----------|-------------|-------------|------------|-----------|-------------|---------------|-------------|--------------------|-------------------|------------|------------|----------------|---------------|-------|
| 1 | 00-00000313 | | Лампа Gauss Filament А60 | | 12W 1200lm 2700К Е27 LED | | | 450 | | шт | 315.63 | 142033.5 | 20% | 23672.25 | 142033.5 | 118361.25 | 23672.25 | Нет | | | | | | Склад осветительных приборов |
| 2 | 00-00000313 | | Лампа Gauss Filament А60 | | 15W 1450lm 4100К Е27 LED | | | 300 | | шт | 396.0 | 118800.0 | 20% | 19800.0 | 118800.0 | 99000.0 | 19800.0 | Нет | | | | | | Склад осветительных приборов |
| 3 | 00-00000313 | | Лампа Gauss Filament А60 | | 20W 1850lm 4100К Е27 LED | | | 385 | | шт | 422.13 | 162520.05 | 20% | 27086.68 | 162520.05 | 135433.37 | 27086.67 | Нет | | | | | | Склад осветительных приборов |
| 4 | 00-00000314 | | Лампа LED ЭРА A60 | | 11W-827-E27 | | | 858 | | шт | 135.0 | 115830.0 | 20% | 19305.0 | 115830.0 | 96525.0 | 19305.0 | Нет | | | | | | Склад осветительных приборов |
| 5 | 00-00000314 | | Лампа LED ЭРА A60 | | 11W-840-E27 | | | 676 | | шт | 135.0 | 91260.0 | 20% | 15210.0 | 91260.0 | 76050.0 | 15210.0 | Нет | | | | | | Склад осветительных приборов |

> **Важно:** Данные начинаются со **2-й строки** (первая строка — заголовки). Используется **первый лист** книги Excel.

**Ключевые колонки для загрузки:**
- **Номенклатура** (колонка D) — наименование товара, поиск в справочнике «Номенклатура»
- **Характеристика** (колонка F) — характеристика номенклатуры, поиск с привязкой к владельцу
- **Количество** (колонка I) — количество единиц товара
- **Ставка НДС** (колонка N) — ставка НДС, поиск в справочнике «Ставки НДС»
- **Склад** (колонка Y) — склад хранения, поиск в справочнике «Склады»

<!-- SCREENSHOT: Скриншот подготовленного Excel-файла -->
> ![Подготовленный Excel-файл](screenshots/01-excel-file.png)

---

## 2. Загрузка файла в систему

1. Откройте веб-интерфейс Loader1C
2. Нажмите кнопку **«Выбрать файл»** или перетащите файл в область загрузки
3. Выберите файл `Ввод остатков товаров.xlsx`
4. Система автоматически определит структуру файла: **25 колонок**, **12 строк данных**

<!-- SCREENSHOT: Скриншот интерфейса загрузки файла -->
> ![Загрузка файла](screenshots/02-file-upload.png)

---

## 3. Выбор типа объекта

1. В выпадающем списке **«Тип объекта»** выберите: **Документ.ВводОстатковТоваров**
2. Система загрузит метаданные документа и его табличных частей

<!-- SCREENSHOT: Скриншот выбора типа объекта -->
> ![Выбор типа объекта](screenshots/03-object-type.png)

---

## 4. Настройка маппинга реквизитов шапки

Для реквизитов шапки документа настройте следующие соответствия:

### 4.1. Реквизиты, заполняемые фиксированным значением

| Реквизит | Способ заполнения | Значение |
|----------|-------------------|----------|
| Дата | Значением | `2026-02-08` |
| ОтражатьВБУиНУ | Значением | `Истина` |
| ОтражатьВОперативномУчете | Значением | `Истина` |
| ОтражатьВУУ | Значением | `Ложь` |
| ОтражатьСебестоимость | Значением | `Истина` |
| ЦенаВключаетНДС | Значением | `Истина` |

### 4.2. Реквизиты, заполняемые предопределенным значением

| Реквизит | Предопределенное значение |
|----------|---------------------------|
| ВидДеятельностиНДС | Продажа облагается НДС |
| НалогообложениеНДС | Продажа облагается НДС |
| ХозяйственнаяОперация | Ввод остатков собственных товаров |

### 4.3. Реквизиты, заполняемые алгоритмом

| Реквизит | Алгоритм (код на 1С) |
|----------|----------------------|
| Префикс | `Результат = "в";` |
| Валюта | `Результат = Справочники.Валюты.НайтиПоНаименованию("RUB")` |
| Организация | `Результат = Справочники.Организации.НайтиПоНаименованию("ЭлектроМир")` |
| Ответственный | `Результат = Пользователи.ТекущийПользователь()` |
| Автор | `Результат = Пользователи.ТекущийПользователь()` |

### 4.4. Реквизиты, заполняемые из файла

| Реквизит | Колонка файла | Способ поиска |
|----------|---------------|---------------|
| Склад | Склад (колонка 24) | По наименованию в справочнике «Склады» |

<!-- SCREENSHOT: Скриншот настройки маппинга шапки -->
> ![Маппинг реквизитов шапки](screenshots/04-header-mapping.png)

---

## 5. Настройка маппинга табличной части «Товары»

Табличная часть **«Товары»** заполняется построчно из Excel-файла. Для каждой строки файла создается строка табличной части.

### 5.1. Реквизиты из файла

| Реквизит ТЧ | Колонка файла | Тип данных | Способ поиска ссылки |
|--------------|---------------|------------|----------------------|
| Номенклатура | Номенклатура (колонка 3) | СправочникСсылка.Номенклатура | По наименованию |
| Характеристика | Характеристика (колонка 5) | СправочникСсылка.ХарактеристикиНоменклатуры | По наименованию + владелец (Номенклатура) |
| Количество | Количество (колонка 8) | Число(15,3) | — |
| КоличествоУпаковок | Количество (колонка 8) | Число(15,3) | — |
| СтавкаНДС | Ставка НДС (колонка 13) | СправочникСсылка.СтавкиНДС | По наименованию |

> **Обратите внимание:** Колонка «Количество» из файла используется для заполнения двух реквизитов: **Количество** и **КоличествоУпаковок**.

### 5.2. Реквизиты, заполняемые алгоритмом

| Реквизит ТЧ | Алгоритм (код на 1С) |
|--------------|----------------------|
| ИдентификаторСтроки | `Результат = Новый УникальныйИдентификатор;` |

### 5.3. Поиск характеристики по владельцу

Для реквизита **Характеристика** настроен составной поиск:
- **Наименование** — берется из колонки «Характеристика» файла (колонка 5)
- **Владелец** — берется из маппинга реквизита «Номенклатура» (маппинг №18)

Это обеспечивает поиск характеристики именно у нужной номенклатуры.

<!-- SCREENSHOT: Скриншот настройки маппинга табличной части -->
> ![Маппинг табличной части «Товары»](screenshots/05-table-mapping.png)

---

## 6. Настройка параметров загрузки

| Параметр | Значение | Описание |
|----------|----------|----------|
| Группировка строк | По колонке **N** (колонка 0) | Строки с одинаковым номером группируются в один документ |
| Лист Excel | Первый лист (индекс 0) | — |
| Начальная строка | 2 | Пропуск строки заголовков |
| Режим обновления | Обновление | Если документ существует — обновить |
| Обязательные поля | Пропускать | Строки с незаполненными обязательными полями пропускаются |
| Использовать транзакцию | Нет | Каждый документ записывается отдельно |
| Режим разработчика | Нет | — |

<!-- SCREENSHOT: Скриншот настройки параметров загрузки -->
> ![Параметры загрузки](screenshots/06-load-settings.png)

---

## 7. Запуск загрузки и проверка результатов

1. Нажмите кнопку **«Загрузить»**
2. Дождитесь завершения процесса
3. Проверьте лог загрузки — убедитесь, что все строки обработаны успешно
4. Откройте созданный документ в 1С и проверьте заполнение реквизитов

<!-- SCREENSHOT: Скриншот процесса загрузки / лога -->
> ![Процесс загрузки](screenshots/07-loading-process.png)

<!-- SCREENSHOT: Скриншот результата — созданный документ в 1С -->
> ![Результат в 1С](screenshots/08-result-in-1c.png)

### Ожидаемый результат

После загрузки будет создан документ **«Ввод остатков товаров»** со следующими данными:

**Шапка документа:**
- Дата: 08.02.2026
- Организация: ЭлектроМир
- Склад: Склад осветительных приборов
- Валюта: RUB
- Хозяйственная операция: Ввод остатков собственных товаров
- Цена включает НДС: Да

**Табличная часть «Товары» (5 строк):**

| Номенклатура | Характеристика | Количество | Ставка НДС |
|-------------|----------------|------------|------------|
| Лампа Gauss Filament А60 | 12W 1200lm 2700К Е27 LED | 450 | 20% |
| Лампа Gauss Filament А60 | 15W 1450lm 4100К Е27 LED | 300 | 20% |
| Лампа Gauss Filament А60 | 20W 1850lm 4100К Е27 LED | 385 | 20% |
| Лампа LED ЭРА A60 | 11W-827-E27 | 858 | 20% |
| Лампа LED ЭРА A60 | 11W-840-E27 | 676 | 20% |

---

## 8. JSON-конфигурация настроек

Ниже приведен полный JSON-файл настроек, который можно импортировать в Loader1C для воспроизведения данного примера.

<details>
<summary>Развернуть JSON-конфигурацию</summary>

```json
{
  "version": "1.0",
  "timestamp": "2026-02-09T15:00:25.530Z",
  "objectType": {
    "fullname": "Документ.ВводОстатковТоваров",
    "name": "ВводОстатковТоваров",
    "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
  },
  "fileStructure": {
    "columns": [
      {
        "dataType": "Число (целое)",
        "index": 0,
        "name": "N",
        "sampleValues": ["1", "2", "3", "4", "5"]
      },
      {
        "dataType": "Строка",
        "index": 1,
        "name": "Код",
        "sampleValues": ["00-00000313", "00-00000313", "00-00000313", "00-00000314", "00-00000314"]
      },
      {
        "dataType": "Число",
        "index": 2,
        "name": "Артикул",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Строка",
        "index": 3,
        "name": "Номенклатура",
        "sampleValues": ["Лампа Gauss Filament А60", "Лампа Gauss Filament А60", "Лампа Gauss Filament А60", "Лампа LED ЭРА A60", "Лампа LED ЭРА A60"]
      },
      {
        "dataType": "Число",
        "index": 4,
        "name": "Назначение",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Строка",
        "index": 5,
        "name": "Характеристика",
        "sampleValues": ["12W 1200lm 2700К Е27 LED", "15W 1450lm 4100К Е27 LED", "20W 1850lm 4100К Е27 LED", "11W-827-E27", "11W-840-E27"]
      },
      {
        "dataType": "Число",
        "index": 6,
        "name": "Серия",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число",
        "index": 7,
        "name": "Статус указания серий",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число (целое)",
        "index": 8,
        "name": "Количество",
        "sampleValues": ["450", "300", "385", "858", "676"]
      },
      {
        "dataType": "Число",
        "index": 9,
        "name": "Упаковка",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Строка",
        "index": 10,
        "name": "Ед. изм.",
        "sampleValues": ["шт", "шт", "шт", "шт", "шт"]
      },
      {
        "dataType": "Число",
        "index": 11,
        "name": "Цена (RUB)",
        "sampleValues": ["315.63", "396.0", "422.13", "135.0", "135.0"]
      },
      {
        "dataType": "Число",
        "index": 12,
        "name": "Сумма (RUB)",
        "sampleValues": ["142033.5", "118800.0", "162520.05", "115830.0", "91260.0"]
      },
      {
        "dataType": "Строка",
        "index": 13,
        "name": "Ставка НДС",
        "sampleValues": ["20%", "20%", "20%", "20%", "20%"]
      },
      {
        "dataType": "Число",
        "index": 14,
        "name": "НДС (RUB)",
        "sampleValues": ["23672.25", "19800.0", "27086.68", "19305.0", "15210.0"]
      },
      {
        "dataType": "Число",
        "index": 15,
        "name": "Сумма с НДС",
        "sampleValues": ["142033.5", "118800.0", "162520.05", "115830.0", "91260.0"]
      },
      {
        "dataType": "Число",
        "index": 16,
        "name": "Сумма (регл.)",
        "sampleValues": ["118361.25", "99000.0", "135433.37", "96525.0", "76050.0"]
      },
      {
        "dataType": "Число",
        "index": 17,
        "name": "НДС (регл)",
        "sampleValues": ["23672.25", "19800.0", "27086.67", "19305.0", "15210.0"]
      },
      {
        "dataType": "Булево",
        "index": 18,
        "name": "Детализация партий",
        "sampleValues": ["Нет", "Нет", "Нет", "Нет", "Нет"]
      },
      {
        "dataType": "Число",
        "index": 19,
        "name": "Номер ГТД / РНПТ",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число",
        "index": 20,
        "name": "Сумма (ПР)",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число",
        "index": 21,
        "name": "Сумма (ВР)",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число",
        "index": 22,
        "name": "Резерв (регл.)",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Число",
        "index": 23,
        "name": "Резерв (упр.)",
        "sampleValues": ["nan", "nan", "nan", "nan", "nan"]
      },
      {
        "dataType": "Строка",
        "index": 24,
        "name": "Склад",
        "sampleValues": ["Склад осветительных приборов", "Склад осветительных приборов", "Склад осветительных приборов", "Склад осветительных приборов", "Склад осветительных приборов"]
      }
    ],
    "fileName": "Ввод остатков товаров.xlsx",
    "fileType": "excel",
    "rowCount": 12
  },
  "mappings": [
    {
      "dataType": "Дата",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "2026-02-08",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "Дата",
      "objectPropertyFullPath": "ВводОстатковТоваров.Дата",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 0,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Дата",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "Число",
      "fileColumn": "Количество",
      "fileColumnIndex": 8,
      "fillMethod": "fromFile",
      "fullType": "Число(15,3)",
      "isRequired": false,
      "objectProperty": "Количество",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.Количество",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": null,
      "referenceProperties": [],
      "searchOrder": 1,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "success",
        "expectedType": "Число",
        "message": ""
      },
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение реквизита\nРезультат = \"в\";",
      "dataType": "Строка",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "Строка(4)",
      "isRequired": false,
      "objectProperty": "Префикс",
      "objectPropertyFullPath": "ВводОстатковТоваров.Префикс",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 2,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Строка",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение строки табличной части\n// СтрокаДанных- обрабатываемая строка файла\nРезультат = Новый УникальныйИдентификатор;",
      "dataType": "Строка",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "Строка(36)",
      "isRequired": false,
      "objectProperty": "ИдентификаторСтроки",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.ИдентификаторСтроки",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 3,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "good",
        "expectedType": "Строка",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение реквизита\nРезультат = Справочники.Валюты.НайтиПоНаименованию(\"RUB\")",
      "dataType": "СправочникСсылка.Валюты",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "СправочникСсылка.Валюты",
      "isRequired": false,
      "objectProperty": "Валюта",
      "objectPropertyFullPath": "ВводОстатковТоваров.Валюта",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 4,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "СправочникСсылка.Валюты",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "dataType": "ПеречислениеСсылка.ТипыНалогообложенияНДС",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "9e3e6bf6-b945-4c3d-9b01-8d19745be442",
      "isPredefined": true,
      "isRequired": false,
      "objectProperty": "ВидДеятельностиНДС",
      "objectPropertyFullPath": "ВводОстатковТоваров.ВидДеятельностиНДС",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": {
        "description": "ПродажаОблагаетсяНДС",
        "displayName": "Продажа облагается НДС",
        "name": "",
        "ref": "9e3e6bf6-b945-4c3d-9b01-8d19745be442",
        "synonym": "Продажа облагается НДС"
      },
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 5,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "ПеречислениеСсылка.ТипыНалогообложенияНДС",
        "message": "Заполнение предопределенным значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "ПеречислениеСсылка.ТипыНалогообложенияНДС",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "9e3e6bf6-b945-4c3d-9b01-8d19745be442",
      "isPredefined": true,
      "isRequired": false,
      "objectProperty": "НалогообложениеНДС",
      "objectPropertyFullPath": "ВводОстатковТоваров.НалогообложениеНДС",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": {
        "description": "ПродажаОблагаетсяНДС",
        "displayName": "Продажа облагается НДС",
        "name": "",
        "ref": "9e3e6bf6-b945-4c3d-9b01-8d19745be442",
        "synonym": "Продажа облагается НДС"
      },
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 6,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "ПеречислениеСсылка.ТипыНалогообложенияНДС",
        "message": "Заполнение предопределенным значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение реквизита\nРезультат = Справочники.Организации.НайтиПоНаименованию(\"ЭлектроМир\")",
      "dataType": "СправочникСсылка.Организации",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "СправочникСсылка.Организации",
      "isRequired": false,
      "objectProperty": "Организация",
      "objectPropertyFullPath": "ВводОстатковТоваров.Организация",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 7,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "СправочникСсылка.Организации",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение реквизита\nРезультат = Пользователи.ТекущийПользователь()",
      "dataType": "СправочникСсылка.Пользователи",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "СправочникСсылка.Пользователи",
      "isRequired": false,
      "objectProperty": "Ответственный",
      "objectPropertyFullPath": "ВводОстатковТоваров.Ответственный",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 8,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "СправочникСсылка.Пользователи",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "dataType": "Булево",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "true",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "ОтражатьВБУиНУ",
      "objectPropertyFullPath": "ВводОстатковТоваров.ОтражатьВБУиНУ",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 9,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Булево",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "Булево",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "true",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "ОтражатьВОперативномУчете",
      "objectPropertyFullPath": "ВводОстатковТоваров.ОтражатьВОперативномУчете",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 10,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Булево",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "Булево",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "false",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "ОтражатьВУУ",
      "objectPropertyFullPath": "ВводОстатковТоваров.ОтражатьВУУ",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 11,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Булево",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "Булево",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "true",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "ОтражатьСебестоимость",
      "objectPropertyFullPath": "ВводОстатковТоваров.ОтражатьСебестоимость",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 12,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Булево",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "СправочникСсылка.Склады",
      "fileColumn": "Склад",
      "fileColumnIndex": 24,
      "fillMethod": "fromFile",
      "fullType": "СправочникСсылка.Склады",
      "isRequired": false,
      "objectProperty": "Склад",
      "objectPropertyFullPath": "ВводОстатковТоваров.Склад",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": "byProperty",
      "referenceProperties": [
        {
          "propertyName": "Наименование",
          "propertyType": "Строка",
          "sourceType": "file",
          "sourceValue": "24"
        }
      ],
      "searchOrder": 13,
      "tablePart": null,
      "validation": {
        "compatibility": "success",
        "expectedType": "СправочникСсылка.Склады",
        "message": ""
      },
      "warnings": []
    },
    {
      "dataType": "ПеречислениеСсылка.ХозяйственныеОперации",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "e026e491-c076-43a4-b9f1-e15a05146b8f",
      "isPredefined": true,
      "isRequired": false,
      "objectProperty": "ХозяйственнаяОперация",
      "objectPropertyFullPath": "ВводОстатковТоваров.ХозяйственнаяОперация",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": {
        "description": "ВводОстатковСобственныхТоваров",
        "displayName": "Ввод остатков собственных товаров",
        "name": "",
        "ref": "e026e491-c076-43a4-b9f1-e15a05146b8f",
        "synonym": "Ввод остатков собственных товаров"
      },
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 14,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "ПеречислениеСсылка.ХозяйственныеОперации",
        "message": "Заполнение предопределенным значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "dataType": "Булево",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "byValue",
      "fillValue": "true",
      "isPredefined": false,
      "isRequired": false,
      "objectProperty": "ЦенаВключаетНДС",
      "objectPropertyFullPath": "ВводОстатковТоваров.ЦенаВключаетНДС",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "predefinedValue": null,
      "referenceMethod": null,
      "searchByGuid": false,
      "searchOrder": 15,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "Булево",
        "message": "Заполнение значением"
      },
      "valueMethod": "predefined",
      "warnings": []
    },
    {
      "algorithm": "// Доступные переменные:\n// Результат - значение реквизита\nРезультат = Пользователи.ТекущийПользователь()",
      "dataType": "СправочникСсылка.Пользователи",
      "fileColumn": null,
      "fileColumnIndex": null,
      "fillMethod": "algorithm",
      "fullType": "СправочникСсылка.Пользователи",
      "isRequired": false,
      "objectProperty": "Автор",
      "objectPropertyFullPath": "ВводОстатковТоваров.Автор",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "searchOrder": 16,
      "tablePart": null,
      "validation": {
        "compatibility": "good",
        "expectedType": "СправочникСсылка.Пользователи",
        "message": "Заполнение алгоритмом"
      },
      "warnings": []
    },
    {
      "dataType": "Число",
      "fileColumn": "Количество",
      "fileColumnIndex": 8,
      "fillMethod": "fromFile",
      "fullType": "Число(15,3)",
      "isRequired": false,
      "objectProperty": "КоличествоУпаковок",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.КоличествоУпаковок",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": null,
      "referenceProperties": [],
      "searchOrder": 17,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "success",
        "expectedType": "Число",
        "message": ""
      },
      "warnings": []
    },
    {
      "dataType": "СправочникСсылка.Номенклатура",
      "fileColumn": "Номенклатура",
      "fileColumnIndex": 3,
      "fillMethod": "fromFile",
      "fullType": "СправочникСсылка.Номенклатура",
      "isRequired": false,
      "objectProperty": "Номенклатура",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.Номенклатура",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": "byProperty",
      "referenceProperties": [
        {
          "propertyName": "Наименование",
          "propertyType": "Строка",
          "sourceType": "file",
          "sourceValue": "3"
        }
      ],
      "searchOrder": 18,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "success",
        "expectedType": "СправочникСсылка.Номенклатура",
        "message": ""
      },
      "warnings": []
    },
    {
      "dataType": "СправочникСсылка.СтавкиНДС",
      "fileColumn": "Ставка НДС",
      "fileColumnIndex": 13,
      "fillMethod": "fromFile",
      "fullType": "СправочникСсылка.СтавкиНДС",
      "isRequired": false,
      "objectProperty": "СтавкаНДС",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.СтавкаНДС",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": "byProperty",
      "referenceProperties": [
        {
          "propertyName": "Наименование",
          "propertyType": "Строка",
          "sourceType": "file",
          "sourceValue": "13"
        }
      ],
      "searchOrder": 19,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "success",
        "expectedType": "СправочникСсылка.СтавкиНДС",
        "message": ""
      },
      "warnings": []
    },
    {
      "dataType": "СправочникСсылка.ХарактеристикиНоменклатуры",
      "fileColumn": "Характеристика",
      "fileColumnIndex": 5,
      "fillMethod": "fromFile",
      "fullType": "СправочникСсылка.ХарактеристикиНоменклатуры",
      "isRequired": false,
      "objectProperty": "Характеристика",
      "objectPropertyFullPath": "ВводОстатковТоваров.Товары.Характеристика",
      "parentObject": {
        "fullname": "Документ.ВводОстатковТоваров",
        "name": "ВводОстатковТоваров",
        "ref": "0a1efc1f-c43e-4cc0-8b14-ad7c677b2b37"
      },
      "referenceMethod": "byProperty",
      "referenceProperties": [
        {
          "propertyName": "Наименование",
          "propertyType": "Строка",
          "sourceType": "file",
          "sourceValue": "5"
        },
        {
          "propertyName": "Владелец",
          "propertyType": "Неизвестно",
          "sourceType": "mapping",
          "sourceValue": "18"
        }
      ],
      "searchOrder": 20,
      "tablePart": {
        "name": "Товары",
        "ref": "db7cc40c-4264-4cea-8d14-d0ee087c4f9a"
      },
      "validation": {
        "compatibility": "success",
        "expectedType": "СправочникСсылка.ХарактеристикиНоменклатуры",
        "message": ""
      },
      "warnings": []
    }
  ],
  "groupingColumn": 0,
  "excelSettings": {
    "sheetIndex": 0,
    "startRow": 2
  },
  "loadSettings": {
    "developerMode": false,
    "useTransaction": false,
    "objectUpdateMode": "update",
    "requiredFieldAction": "skip",
    "customAlgorithms": [],
    "modifySearchProperties": []
  }
}
```

</details>

---

## Частые вопросы

### Как загрузить данные в несколько документов?

Используйте колонку группировки. В данном примере группировка настроена по колонке **N** (порядковый номер). Все строки с одинаковым номером попадут в один документ. Если вам нужно разбить данные на несколько документов — используйте колонку с уникальным идентификатором группы (например, номер партии или код склада).

### Что произойдет, если номенклатура не найдена в справочнике?

Поведение зависит от настройки **«Обязательные поля»**. В данном примере выбрано значение **«Пропускать»** — строки с ненайденной номенклатурой будут пропущены, а информация об ошибке отобразится в логе загрузки.

### Как работает поиск характеристики по владельцу?

Характеристика ищется не просто по наименованию, а с учетом владельца (номенклатуры). Это предотвращает ситуацию, когда у разных товаров есть характеристики с одинаковыми наименованиями. В маппинге указана связь: свойство «Владелец» берется из маппинга №18 (Номенклатура).
