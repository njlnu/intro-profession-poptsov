# intro-profession-poptsov

## Практическая работа № 2. Работа с LLM через API

Вариант 19, 15 заявок из `rental_19.csv`. Ноутбуки отправляют HTTP-запросы к локальному
серверу llama.cpp с моделью Qwen3-4B (Q4_K_M). Для этого API ключ не требуется.

| Ветка | Ноутбук | Содержание |
| --- | --- | --- |
| `env-feature` | `llm_demo.ipynb` | Проверка API и базовое извлечение числа проживающих |
| `main`, `llm-improve-1` | `llm_work.ipynb` | Задание № 1 и оценка точности по `amount` |
| `llm-improve-2` | `llm_work.ipynb` | Оба задания, структурированный JSON и сравнение с ручной разметкой |

### Запуск на Windows

Из корня репозитория создайте среду Python 3.13 и установите зависимости.

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install --no-cache-dir -r requirements-lab2.txt
```

Поместите [файл модели Qwen3-4B-Q4_K_M.gguf](https://huggingface.co/Qwen/Qwen3-4B-GGUF/blob/main/Qwen3-4B-Q4_K_M.gguf)
в `.local-llm`. Распакуйте [сборку llama.cpp для Windows x64](https://github.com/ggml-org/llama.cpp/releases/tag/b12549)
в `.local-llm/llama`. Для видеокарты используется сборка Vulkan. Для запуска на процессоре
выберите сборку CPU и замените `--n-gpu-layers 99` на `--n-gpu-layers 0`.

Из корня репозитория запустите сервер и оставьте терминал открытым на время прогона.

```powershell
& .\.local-llm\llama\llama-server.exe `
  --model .\.local-llm\Qwen3-4B-Q4_K_M.gguf --alias Qwen3-4B-Q4_K_M `
  --host 127.0.0.1 --port 18765 --ctx-size 4096 --parallel 1 --n-gpu-layers 99
```

Выберите `.venv` в качестве ядра Jupyter и выполните все ячейки ноутбука. Базовый прогон
записывается в `rental_with_results.csv`. На ветке `llm-improve-2` файл
`rental_structured_results.csv` содержит ответы модели, ручную разметку и отметки совпадений.
