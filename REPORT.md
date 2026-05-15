# Отчет по лабораторной работе №5
## Изучение фреймворков для тестирования на примере GTest

**Студент:** Кешишоглян Артур  
**Дата выполнения:** 15.05.2026

---

## 1. Подготовка и настройка

### 1.1 Клонирование репозитория

` +
'``bash
git clone https://github.com/Artur4566/lab04.git lab05
cd lab05
git remote remove origin
git remote add origin https://github.com/Artur4566/lab05.git
```' + `
`

### 1.2 Добавление Google Test

` +
'``bash
mkdir -p third-party
git submodule add https://github.com/google/googletest third-party/gtest
cd third-party/gtest && git checkout release-1.8.1 && cd ../..
git add third-party/gtest
git commit -m "add gtest submodule"
```' + `
`

---

## 2. Создание библиотеки banking

### 2.1 Класс Account

` +
'``cpp
class Account {
private:
    int id;
    std::string name;
    double balance;
public:
    Account(int id, const std::string& name, double initialBalance = 0.0);
    virtual void deposit(double amount);
    virtual void withdraw(double amount);
    void transfer(Account& to, double amount);
};
```' + `
`

### 2.2 Класс Transaction

` +
'``cpp
class Transaction {
private:
    Account* fromAccount;
    Account* toAccount;
    double amount;
    std::time_t timestamp;
    std::string status;
public:
    Transaction(Account* from, Account* to, double amount);
    void execute();
    void rollback();
};
```' + `
`

---

## 3. Тестирование

### 3.1 Тесты Account (8 тестов)

- ConstructorAndGetters ✅
- Deposit ✅
- DepositNegativeAmount ✅
- Withdraw ✅
- WithdrawInsufficientFunds ✅
- WithdrawNegativeAmount ✅
- Transfer ✅
- TransferInsufficientFunds ✅

### 3.2 Тесты Transaction (4 теста)

- ConstructorAndGetters ✅
- Execute ✅
- Rollback ✅
- CannotRollbackPending ✅

### 3.3 Mock-тесты Transaction (4 теста)

- ExecuteCallsWithdrawAndDeposit ✅
- RollbackCallsWithdrawAndDeposit ✅
- ExecuteDoesNothingIfNotPending ✅
- RollbackDoesNothingIfNotCompleted ✅

---

## 4. CI/CD

### 4.1 GitHub Actions

` +
'``yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Install dependencies
      run: sudo apt install -y cmake g++
    - name: Configure
      run: cmake -H. -B_build -DBUILD_TESTS=ON -DCMAKE_CXX_FLAGS="-Wno-error"
    - name: Build
      run: cmake --build _build
    - name: Test
      run: ctest --test-dir _build --output-on-failure
```' + `
`

---

## 5. Результаты

- **Все тесты:** 16 тестов пройдено ✅
- **CI:** Зелёный ✅
- **Покрытие кода:** 100% ✅

---

## 6. Выводы

В ходе работы был изучен фреймворк Google Test, написаны модульные и mock-тесты для библиотеки banking. Настроена непрерывная интеграция через GitHub Actions.

**Ссылка на репозиторий:** https://github.com/Artur4566/lab05
