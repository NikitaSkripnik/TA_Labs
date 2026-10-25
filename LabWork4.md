Лістинг 1.1 - Хешування з ланцюжками
```python
M = 13

WORDS = ["ШАНУЙ", "БАТЬКА", "Й", "МАТІР", "ТО",
         "БУДЕ", "ТОБІ", "В", "ЖИТТІ", "ДОБРЕ"]

LETTER_POSITIONS = {
    'А': 1,  'Б': 2,  'В': 3,  'Г': 4,  'Ґ': 5,
    'Д': 6,  'Е': 7,  'Є': 8,  'Ж': 9,  'З': 10,
    'И': 11, 'І': 12, 'Ї': 13, 'Й': 14, 'К': 15,
    'Л': 16, 'М': 17, 'Н': 18, 'О': 19, 'П': 20,
    'Р': 21, 'С': 22, 'Т': 23, 'У': 24, 'Ф': 25,
    'Х': 26, 'Ц': 27, 'Ч': 28, 'Ш': 29, 'Щ': 30,
    'Ь': 31, 'Ю': 32, 'Я': 33
}


def simple_hash_from_map(key: str) -> int:
    sum_of_positions = 0
    for char in key:
        position = LETTER_POSITIONS.get(char, 0)
        sum_of_positions += position
    hash_address = sum_of_positions % M
    return hash_address


def build_open_hash_table(words: list, m: int) -> list:
    hash_table = [[] for _ in range(m)]
    for word in words:
        address = simple_hash_from_map(word)
        hash_table[address].append(word)

    return hash_table


def display_hash_table(table: list):
    print("\n--- Результат хешування ---")
    for i, chain in enumerate(table):
        print(f"Індекс {i:02d}: {chain}")

hash_table = build_open_hash_table(WORDS, M)
display_hash_table(hash_table)
```

Результат:
```
--- Результат хешування ---
Індекс 00: ['БУДЕ', 'ЖИТТІ']
Індекс 01: ['Й']
Індекс 02: []
Індекс 03: ['ТО', 'В', 'ДОБРЕ']
Індекс 04: ['ТОБІ']
Індекс 05: []
Індекс 06: []
Індекс 07: []
Індекс 08: ['ШАНУЙ', 'БАТЬКА']
Індекс 09: ['МАТІР']
Індекс 10: []
Індекс 11: []
Індекс 12: []
```

Лістинг 1.2 - Хешування з ланцюжками
```python
M = 13

WORDS = ["ШАНУЙ", "БАТЬКА", "Й", "МАТІР", "ТО",
         "БУДЕ", "ТОБІ", "В", "ЖИТТІ", "ДОБРЕ"]

LETTER_POSITIONS = {
    'А': 1,  'Б': 2,  'В': 3,  'Г': 4,  'Ґ': 5,
    'Д': 6,  'Е': 7,  'Є': 8,  'Ж': 9,  'З': 10,
    'И': 11, 'І': 12, 'Ї': 13, 'Й': 14, 'К': 15,
    'Л': 16, 'М': 17, 'Н': 18, 'О': 19, 'П': 20,
    'Р': 21, 'С': 22, 'Т': 23, 'У': 24, 'Ф': 25,
    'Х': 26, 'Ц': 27, 'Ч': 28, 'Ш': 29, 'Щ': 30,
    'Ь': 31, 'Ю': 32, 'Я': 33
}

def primary_hash(key: str) -> int:
    sum_of_positions = 0
    for char in key:
        position = LETTER_POSITIONS.get(char, 0)
        sum_of_positions += position
    return sum_of_positions % M


def build_closed_hash_table(words: list, m: int) -> list:
    hash_table = [None] * m
    inserted_count = 0

    for word in words:
        start_address = primary_hash(word)
        address = start_address

        for i in range(m):

            address = (start_address + i) % m

            if hash_table[address] is None:

                hash_table[address] = word
                inserted_count += 1
                break
        else:
            print(f"Помилка: Таблиця заповнена. Не вдалося додати слово: {word}")

    return hash_table

def display_hash_table(table: list):
    print("\n--- Хеш-таблиця ---")
    print("Індекс | Слово")
    print("-------|-------")
    for i, item in enumerate(table):
        value = item if item is not None else "(NULL)"
        print(f"{i:02d} | {value}")

hash_table = build_closed_hash_table(WORDS, M)
display_hash_table(hash_table)
```

Результат:
```
--- Хеш-таблиця ---
Індекс | Слово
-------|-------
00 | БУДЕ
01 | Й
02 | ЖИТТІ
03 | ТО
04 | ТОБІ
05 | В
06 | ДОБРЕ
07 | (NULL)
08 | ШАНУЙ
09 | БАТЬКА
10 | МАТІР
11 | (NULL)
12 | (NULL)
```
