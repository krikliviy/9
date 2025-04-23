from rational import Rational


class RationalList:
    def __init__(self, initial_elements=None):
        self.elements = []
        if initial_elements is not None:
            for elem in initial_elements:
                self.add(elem)

    def add(self, element):
        if isinstance(element, int):
            element = Rational(element, 1)
        elif not isinstance(element, Rational):
            raise TypeError("допустимі тільки int або Rational")
        self.elements.append(element)

    def __getitem__(self, index):
        return self.elements[index]

    def __setitem__(self, index, element):
        if isinstance(element, int):
            element = Rational(element, 1)
        elif not isinstance(element, Rational):
            raise TypeError("допустимі тільки int або Rational")
        self.elements[index] = element

    def __len__(self):
        return len(self.elements)

    def __add__(self, other):
        new_list = RationalList(self.elements)
        if isinstance(other, RationalList):
            for item in other:
                new_list.add(item)
        elif isinstance(other, (int, Rational)):
            new_list.add(other)
        else:
            raise TypeError("можна додавати тільки RationalList або число")
        return new_list

    def __radd__(self, other):
        if isinstance(other, (int, Rational)):
            return RationalList([other]) + self
        raise TypeError("можна додавати тільки RationalList або число")

    def __iadd__(self, other):
        if isinstance(other, RationalList):
            for item in other:
                self.add(item)
        elif isinstance(other, (int, Rational)):
            self.add(other)
        else:
            raise TypeError("можна додавати тільки RationalList або число")
        return self

    def __iter__(self):
        return iter(self.elements)


def convert_to_rational(token):
    token = token.strip()
    if '/' in token:
        return Rational(token)
    return Rational(int(token), 1)


def load_rational_list(file_path):
    rat_list = RationalList()
    with open(file_path, encoding='utf-8') as file:
        for line in file:
            for token in line.split():
                rat_list.add(convert_to_rational(token))
    return rat_list


def total_sum(r_list):
    sum_result = Rational(0, 1)
    for fraction in r_list:
        sum_result = sum_result + fraction
    return sum_result


if __name__ == "__main__":
    filenames = ["input01", "input02", "input03"]
    for name in filenames:
        try:
            rl = load_rational_list(name)
            summation = total_sum(rl)
            print(f"{name} {summation} {summation()}")
        except Exception as err:
            print(f"{name} {err}")
