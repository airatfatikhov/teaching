#### Полиморфизм — сила наследования

Полиморфизм позволяет работать с объектами разных классов единообразно, если они наследуются от общего предка.
```
class Employee:
    def __init__(self, name):
        self.name = name

    def get_salary_report(self):
        return f"{self.name}: базовая зарплата"


class Developer(Employee):
    def get_salary_report(self):
        return f"{self.name}: зарплата + бонус за код"


class Designer(Employee):
    def get_salary_report(self):
        return f"{self.name}: зарплата + бонус за дизайн"


# Функция работает с ЛЮБЫМ сотрудником — ей не важен конкретный класс
def print_salary(employee):
    # Вызов метода, который зависит от реального типа объекта
    report = employee.get_salary_report()
    print(report)


# Создаем разных сотрудников
staff = [
    Employee("Директор"),
    Developer("Программист"),
    Designer("Дизайнер")
]

# Один и тот же код работает для всех!
for person in staff:
    print_salary(person)
```

Вывод:

```
Директор: базовая зарплата
Программист: зарплата + бонус за код
Дизайнер: зарплата + бонус за дизайн
```