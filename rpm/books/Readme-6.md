#### 🎭 Инкапсуляция — это контракт

Представьте, что вы садитесь в машину. Вам не нужно знать, как работает двигатель, коробка передач или система впрыска топлива. Вам достаточно:
* Повернуть ключ (или нажать кнопку)
* Нажать на педаль газа
* Крутить руль

Машина скрывает сложность и предоставляет простой интерфейс. Это и есть инкапсуляция в реальной жизни.

##### ❌ Без инкапсуляции (плохо)
```
class User:
    def __init__(self, name, age):
        self.name = name
        self.age = age

user = User("Иван", 25)

# Любой код может сломать объект!
user.age = -150       # ❌ Отрицательный возраст? Легко!
user.age = "двадцать" # ❌ Строка вместо числа? Без проблем!
user.name = []        # ❌ Список вместо имени? Почему бы и нет!
```

##### ✅ С инкапсуляцией (хорошо)

```
class User:
    def __init__(self, name, age):
        self.name = name
        self.set_age(age)  # Используем сеттер с валидацией

    def set_age(self, age):
        if not isinstance(age, int):
            raise TypeError("Возраст должен быть числом")
        if age < 0 or age > 150:
            raise ValueError("Возраст должен быть от 0 до 150")
        self.__age = age  # Приватный атрибут

    def get_age(self):
        return self.__age

user = User("Иван", 25)
user.set_age(30)          # ✅ Работает
# user.set_age(-150)      # 💥 ValueError: Возраст должен быть от 0 до 150
# user.set_age("двадцать") # 💥 TypeError: Возраст должен быть числом
# user.__age              # 💥 AttributeError: нет такого атрибута
```

Пример всех трех уровней

```
class Account:
    def __init__(self, owner, balance, pin):
        self.owner = owner          # ✅ Public — можно читать/менять откуда угодно
        self._bank = "Сбербанк"     # ⚠️ Protected — для внутреннего использования
        self.__pin = pin            # 🔒 Private — строго внутри класса
        self.__balance = balance    # 🔒 Private

    def get_balance(self):
        return self.__balance

    def _check_pin(self, pin):
        """Protected метод — для служебных нужд"""
        return self.__pin == pin

    def withdraw(self, amount, pin):
        """Публичный метод — использует приватные данные"""
        if not self._check_pin(pin):
            raise PermissionError("Неверный PIN")
        if amount > self.__balance:
            raise ValueError("Недостаточно средств")
        self.__balance -= amount
        return self.__balance


acc = Account("Иван", 10000, 1234)

# ✅ Public — всё доступно
print(acc.owner)           # Иван
acc.owner = "Пётр"         # Работает

# ⚠️ Protected — формально доступно, но не рекомендуется
print(acc._bank)           # Сбербанк
acc._bank = "Тинькофф"     # Работает, но это "дурной тон"

# 🔒 Private — НЕ доступно извне
# print(acc.__pin)         # 💥 AttributeError
# print(acc.__balance)     # 💥 AttributeError

# ✅ Но можно через публичные методы
print(acc.get_balance())   # 10000
acc.withdraw(5000, 1234)   # 5000
```