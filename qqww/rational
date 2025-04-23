import math


class Rational:
    def __init__(self, numerator, denominator=None):
        if denominator is None:
            if isinstance(numerator, str):
                try:
                    num_str, den_str = numerator.split('/')
                    self.num = int(num_str)
                    self.den = int(den_str)
                except Exception as err:
                    raise ValueError("Невірний формат рядка. Має бути 'n/d'") from err
            elif isinstance(numerator, Rational):
                self.num = numerator.num
                self.den = numerator.den
            else:
                raise ValueError("Непідтримуваний тип аргументів")
        else:
            if isinstance(numerator, int) and isinstance(denominator, int):
                self.num = numerator
                self.den = denominator
            else:
                raise ValueError("Чисельник і знаменник мають бути цілими числами")
        if self.den == 0:
            raise ZeroDivisionError("Знаменник не може дорівнювати нулю")
        if self.den < 0:
            self.num = -self.num
            self.den = -self.den
        self._simplify()

    def _simplify(self):
        common_divisor = math.gcd(self.num, self.den)
        if common_divisor:
            self.num //= common_divisor
            self.den //= common_divisor

    def __str__(self):
        return f"{self.num}/{self.den}"

    def __repr__(self):
        return f"Rational({self.num}, {self.den})"

    def __add__(self, other):
        if isinstance(other, int):
            other = Rational(other, 1)
        if isinstance(other, Rational):
            new_num = self.num * other.den + other.num * self.den
            new_den = self.den * other.den
            return Rational(new_num, new_den)
        raise TypeError("Додавання можливе лише з типом int або Rational")

    def __sub__(self, other):
        if isinstance(other, int):
            other = Rational(other, 1)
        if isinstance(other, Rational):
            new_num = self.num * other.den - other.num * self.den
            new_den = self.den * other.den
            return Rational(new_num, new_den)
        raise TypeError("Віднімання можливе лише з типом int або Rational")

    def __mul__(self, other):
        if isinstance(other, int):
            other = Rational(other, 1)
        if isinstance(other, Rational):
            return Rational(self.num * other.num, self.den * other.den)
        raise TypeError("Множення можливе лише з типом int або Rational")

    def __truediv__(self, other):
        if isinstance(other, int):
            if other == 0:
                raise ZeroDivisionError("Ділення на нуль")
            other = Rational(other, 1)
        if isinstance(other, Rational):
            if other.num == 0:
                raise ZeroDivisionError("Ділення на нуль")
            return Rational(self.num * other.den, self.den * other.num)
        raise TypeError("Ділення можливе лише з типом int або Rational")

    def __call__(self):
        return self.num / self.den

    def __getitem__(self, key):
        if key == "n":
            return self.num
        if key == "d":
            return self.den
        raise KeyError("Ключ має бути 'n' для чисельника або 'd' для знаменника")

    def __setitem__(self, key, value):
        if not isinstance(value, int):
            raise ValueError("Нове значення має бути цілим числом")
        if key == "n":
            self.num = value
        elif key == "d":
            if value == 0:
                raise ZeroDivisionError("Знаменник не може бути 0")
            self.den = value
        else:
            raise KeyError("Ключ має бути 'n' або 'd'")
        self._simplify()


def evaluate_expression(expression: str) -> Rational:
    expr_str = expression.replace(" ", "")
    index = 0

    def parse_number():
        nonlocal index
        start_index = index
        while index < len(expr_str) and (expr_str[index].isdigit() or expr_str[index] == '/'):
            index += 1
        token = expr_str[start_index:index]
        return Rational(token) if '/' in token else Rational(int(token), 1)

    def parse_factor():
        nonlocal index
        if index < len(expr_str) and expr_str[index] == '-':
            index += 1
            return Rational(0, 1) - parse_factor()
        if index < len(expr_str) and expr_str[index] == '(':
            index += 1  # Пропускаємо відкриваючу дужку
            result = parse_expr()
            if index >= len(expr_str) or expr_str[index] != ')':
                raise ValueError("Відсутня закриваюча дужка")
            index += 1  # Пропускаємо закриваючу дужку
            return result
        return parse_number()

    def parse_term():
        nonlocal index
        result = parse_factor()
        while index < len(expr_str) and expr_str[index] in "*/":
            op = expr_str[index]
            index += 1
            if op == '*':
                result = result * parse_factor()
            elif op == '/':
                result = result / parse_factor()
        return result

    def parse_expr():
        nonlocal index
        result = parse_term()
        while index < len(expr_str) and expr_str[index] in '+-':
            op = expr_str[index]
            index += 1
            if op == '+':
                result = result + parse_term()
            else:
                result = result - parse_term()
        return result

    final_result = parse_expr()
    if index != len(expr_str):
        raise ValueError("Невірний формат виразу")
    return final_result


if __name__ == "__main__":
    test_expressions = [
        "4 - 92 - 79 * 59 * 90/16 * 75 - 55 * 82/41 * 19",
        "48 + 74/40 * 64 * 93/50 * 52/77 * 57 * 45/95 * 30 * 77/20 * 74 * 59/27 + 29 + 18 * 84/19 - 84/73 + 56/66 - 62 - 99 + 9 * 8 + 71/19 * 51 * 35 * 29 + 86/80 + 45 * 42 * 92 * 98/78 * 33/92 + 70",
        "9 * 40 + 96 * 83 - 43 - 69 + 12 * 48/65 * 10 - 90"
    ]
    for idx, exp in enumerate(test_expressions, start=1):
        try:
            result_obj = evaluate_expression(exp)
            decimal_val = result_obj()
            print(f"Вираз {idx}: {exp}")
            print(f"  Результат як дріб: {result_obj}")
            print(f"  Десятковий результат: {decimal_val:.3f}\n")
        except Exception as exc:
            print(f"Помилка в обчисленні виразу {idx}: {exc}\n")
