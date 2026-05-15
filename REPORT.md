# Отчет по лабораторной работе №4
## Тема: Настройка систем непрерывной интеграции (CI/CD)

**Студент:** Кешишоглян Артур  
**Дата выполнения:** 15.05.2026

---

## 1. Выполнение туториала

### 1.1 Настройка переменных окружения

```bash
export GITHUB_USERNAME=Artur4566
export GITHUB_TOKEN=ghp...
```

### 1.2 Клонирование проекта

```bash
git clone https://github.com/Artur4566/lab03.git projects/lab04
cd projects/lab04
git remote remove origin
git remote add origin https://github.com/Artur4566/lab04.git
```

### 1.3 Создание .travis.yml

```yaml
language: cpp

script:
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build
- cmake --build _build --target install

addons:
  apt:
    sources:
      - george-edison55-precise-backports
    packages:
      - cmake
      - cmake-data
```

### 1.4 Установка Travis CLI

```bash
gem install travis
travis login --github-token ${GITHUB_TOKEN}
travis lint
```

> **Примечание:** Travis CI не использовался из-за отсутствия бесплатных планов для новых пользователей. Вместо него настроен GitHub Actions, который полностью заменяет его функционал.

---

## 2. Домашнее задание

### 2.1 GitHub Actions (Linux, gcc/clang)

```yaml
name: CI

on: [push, pull_request]

jobs:
  build-linux:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        compiler: [g++, clang++]
    steps:
    - uses: actions/checkout@v4
    - name: Configure
      run: |
        mkdir build && cd build
        cmake .. -DCMAKE_CXX_COMPILER=${{ matrix.compiler }}
    - name: Build
      run: cmake --build build
```

### 2.2 AppVeyor (Windows)

```yaml
version: 1.0.{build}
image: Visual Studio 2022

platform: x64
configuration: [Debug, Release]

build_script:
  - mkdir build
  - cd build
  - cmake ..
  - cmake --build . --config %CONFIGURATION%

test_script:
  - ctest -C %CONFIGURATION% --output-on-failure
```

### 2.3 Бейджи в README

```markdown
[![CI](https://github.com/Artur4566/lab04/actions/workflows/ci.yml/badge.svg)](https://github.com/Artur4566/lab04/actions/workflows/ci.yml)
[![Build status](https://ci.appveyor.com/api/projects/status/github/Artur4566/lab04?svg=true)](https://ci.appveyor.com/project/Artur4566/lab04)
```

---

## 3. Статус сборок

- **GitHub Actions (Linux, gcc/clang):** ✅ Успешно
- **AppVeyor (Windows):** ✅ Успешно

---

## 4. Выводы

В ходе работы настроены:
- GitHub Actions для Linux (gcc и clang)
- AppVeyor для Windows

Travis CI не использовался из-за отсутствия бесплатных планов. GitHub Actions полностью заменяет его функционал.

**Ссылка на репозиторий:** https://github.com/Artur4566/lab04
```
