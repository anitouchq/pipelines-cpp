# 🧱 pipelines-cpp

## 📖 О проекте

Простое C++-приложение (`getGreeting`, `main`), для которого настроен CI-пайплайн.
При каждом `push` и `pull request` в ветки `main` / `master` автоматически:

- 🧹 проверяется форматирование через **clang-format**
- 🏗️ собирается проект через **CMake**
- ✅ прогоняются тесты через **Google Test (gtest)**
- 🐳 собирается **Docker-образ** (multi-stage: Ubuntu → Ubuntu) и сохраняется как артефакт

---

## 📂 Структура проекта

```
.
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions workflow
├── src/
│   └── main.cpp            # основной код (getGreeting, main)
├── tests/
│   └── test.cpp            # тесты (Google Test)
├── CMakeLists.txt          # сборка CMake
├── Dockerfile              # multi-stage: builder → runtime
├── .dockerignore           # что не копировать в образ
└── README.md
```

---

## ⚙️ Как работает пайплайн

| Job | Шаг | Что делает |
|-----|-----|-----------|
| **format** | clang-format | Проверяет стиль `src/` |
| **build-and-test** | Install deps | Ставит `cmake`, `libgtest-dev`, собирает gtest |
| | Configure | `cmake -B build -DBUILD_TESTS=ON` |
| | Build | `cmake --build build` |
| | Test | `ctest --output-on-failure` |
| **docker-build** | Buildx | Сборка образа с кэшем GHA |
| | Save + Upload | Сохраняет образ как артефакт (`docker-image.tar.gz`) |
| | Test | `docker run --rm my-cpp-app:latest` |

---

## 🐳 Проверка локально

### Сборка образа
<img width="2170" height="725" alt="photo_5413821073230340029_w" src="https://github.com/user-attachments/assets/c4993bd6-af4a-449c-946a-fcdf5cd82d66" />


```bash
docker build -t my-cpp-app:latest .
```

### Запуск контейнера

```bash
docker run --rm my-cpp-app:latest
```

Ожидаемый вывод:

```
Hello from C++ in Docker! 🐳
```

---

## 🧪 Сборка и тесты локально

```bash
cmake -B build -DBUILD_TESTS=ON
cmake --build build
cd build && ctest --output-on-failure
```

---

## 🏗️ Стек

- **C++ 17**
- **CMake 3.16+** — сборка
- **Google Test** — тесты
- **clang-format 17** — форматирование
- **Docker** — multi-stage сборка

---

## 📄 Лицензия

MIT — свободно для учебных целей.
