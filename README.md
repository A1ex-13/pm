# pm
``` python
import requests
import csv
from difflib import SequenceMatcher

# Функция для загрузки данных с GitHub
def load_regions_from_github(url):
    response = requests.get(url)
    response.raise_for_status()
    decoded_content = response.content.decode('utf-8').splitlines()
    return list(csv.reader(decoded_content))

# Функция для поиска индекса
def find_postal_code(input_str, regions):
    def similarity(a, b): return SequenceMatcher(None, a.lower(), b.lower()).ratio()
    best_match = max(regions, key=lambda r: max(similarity(input_str, r[0]), similarity(input_str, r[1] or "")))
    return best_match[2] if similarity(input_str, best_match[0]) > 0.75 else "800"

# Основной код
url = "https://raw.githubusercontent.com/yourusername/yourrepo/main/regions.csv"
regions = load_regions_from_github(url)[1:]  # Пропуск заголовка
while True:
    query = input("Введите регион: ").strip()
    if query.lower() == 'exit': break
    print("Индекс:", find_postal_code(query, regions))
```
