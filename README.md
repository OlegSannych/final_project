'''Итоговый проект.
Реализовать графическую программу с использованием библиотеки Tkinter, которая состоит из поля ввода текста, раскрывающегося списка и полем вывода надписи и кнопки.
Логика работы программы:
1.Пользователь вводит последовательность чисел через запятую
2.Выбирает один из вариантов сортировки
3.После нажатия на кнопку Start происходит сортировка последовательности с последующим выводом в текстовое поле вывода
4.Реализовать вывод времени затраченного на сортировку
Код должен содержать комментарии, а также все необходимые проверки на исключения.
Код должен быть максимально читаем. При написании следует прибегнуть к стандартным библиотекам тестирования!'''



import tkinter as tk
from tkinter import ttk
import time
import unittest


class Program:
    '''Program это основной класс программы  для сортировки чисел в классе используются методы:
    __init__ - конструктор класса
    creating_placing_widgets - для создания и размещение виджетов
    sorted_numbers - для сортировки чисел
    pressing_start_button - для обработки действия при нажатии кнопки Start
    show_window используется для запуска основного цикла обработки событий окна
    '''

    # Конструктор класса
    def __init__(self, test_mode=False):
        # Создаем окно если это не тестирование
        if not test_mode:
            self.window = tk.Tk()
            self.window.title('Программа для сортировки чисел')
            # Вызываем метод создания и размещения виджетов в окне
            self.creating_placing_widgets()

    # Метод для создания и размещения виджетов
    def creating_placing_widgets(self):
        '''Метод creating_placing_widgets предназначен для создания и размещения виджетов по заданным параметрам
        используются методы из модуля tkinter
        Label -используется для создания текстовой метки, которая может отображать статичный текст или изображение.
        Entry -используется для создания поля ввода текста
        Combobox -используется для создания выпадающего списока с возможностью ввода текста
        Button - используется для создания кнопки
        pack - используется для упаковки и размещения виджетов в окне
        set - используется с виджетом Combobox, для установки значения по умолчанию.'''
        # Создаем виджеты
        self.input_entry = tk.Entry(self.window, width=50)
        self.combobox = ttk.Combobox(self.window, values=["По возрастанию", "По убыванию"])
        self.output_label = tk.Label(self.window, text="Отсортированный список:")
        self.time_label = tk.Label(self.window, text="Время сортировки:")
        start_button = tk.Button(self.window, text="Start", command=self.pressing_start_button)
        title_label = tk.Label(self.window, text='Программа для сортировки чисел', font=("Arial", 12))
        input_label = tk.Label(self.window, text='Введите числа через запятую: ')
        sort_method_label = tk.Label(self.window, text="Выберите метод сортировки:")

        # Размещаем виджеты
        title_label.pack(pady=5)
        input_label.pack(pady=5)
        self.input_entry.pack(padx=20, pady=20)
        sort_method_label.pack(pady=5)
        self.combobox.pack(pady=5)
        start_button.pack(pady=20)
        # Установка значения по умолчанию
        self.combobox.set("По возрастанию")
        self.output_label.pack(pady=5)
        self.time_label.pack(pady=5)

    # Метод для сортировки чисел
    def sorted_numbers(self, numbers, method):
        '''sorted_numbers принимает список из чисел и параметр задающий тип сортировки.
        Сортирует список согласно заданному параметру
        Для сортировки используется встроенный метод sorted'''
        if method == "По возрастанию":
            return sorted(numbers)
        elif method == "По убыванию":
            return sorted(numbers, reverse=True)

    # Метод для обработки действия при нажатии кнопки Start
    def pressing_start_button(self):
        '''Метод pressing_start_button вызывается при нажатии пользователем кнопки Start.
        В методе производится проверка введенных чисел и выбранного метода, создается список из чисел для сортировки,
        вызывается метод сортировки, выводится отсортированный список и время затраченное на сортировку метод использует:
        try- проверка на исключения , get-получение текущего значения виджета, split- задает разделитель в строке,
        strip- удаляет пробелы в начале и конце строки, time- функция для получения текущего времени,
        config-изменяет свойства виджета(для замены текста в output_label),
        '''
        try:
            # Получаем данные из input_entry, преобразуем в список по разделителю ","
            number_sequence = self.input_entry.get().split(',')
            # убираем лишние пробелы преобразуем  элементы списка в целые числа
            numbers = [int(num.strip()) for num in number_sequence]
            # Сохраняем текущее значение combobox в переменную
            method = self.combobox.get()
            # Проверяем корректность выбора метода
            if method not in ["По возрастанию", "По убыванию"]:
                # Вызываем исключения в случае некорректного ввода метода
                raise ValueError("Ошибка: Некорректный метод сортировки.")
            # Определяем время старта сортировки(текущее время)
            start_time = time.perf_counter()
            # Вызываем метод сортировки для получения отсортированного списка
            sorted_list = self.sorted_numbers(numbers, method)
            # Определяем время сортировки путем вычисления разницы текущего времени и времени старта
            elapsed_time = time.perf_counter() - start_time
            # Изменяем текст output_label выводя на экран отсортированный список
            self.output_label.config(text=f"Отсортированный список: {sorted_list}")
            # Изменяем текст time_label выводя на экран затраченное время на сортировку
            self.time_label.config(text=f"Время сортировки: {elapsed_time:.6f} сек")
        # Исключение  по ошибкам формата и условий значения
        except ValueError as e:
            # Проверяем сообщение ошибки для вывода подсказкой о возникших исключениях
            if "Ошибка: Некорректный метод сортировки." in str(e):
                self.output_label.config(text=str(e))
            else:
                self.output_label.config(text="Ошибка: Введите числа через запятую.")
        # Дополнительное исключение при вознинковении непредвиденной ошибки
        except Exception as e:
            self.output_label.config(text="Произошла неожиданная ошибка.")

    # Метод show_window используется для запуска основного цикла обработки событий окна
    def show_window(self):
        ''' Метод show_window используется для запуска основного цикла обработки событий окна'''
        self.window.mainloop()


class TestSorting(unittest.TestCase):
    ''' Класс TestSorting предназначен для тестирования сортировки, с помощью встроенного метода assertEqual
    проверяем списки на совпадение'''

    # Создаем экземпляр класса для проверки
    def setUp(self):
        self.program = Program(test_mode=True)

    # Тестируем сортировку по возрастанию
    def test_sort_ascending(self):
        # Создаем список для проверки
        data = [3, 2, 1]
        # Желаемый результат
        expected = [1, 2, 3]
        self.assertEqual(self.program.sorted_numbers(data, "По возрастанию"), expected)

    # Тестируем сортировку по убыванию
    def test_sort_descending(self):
        # Создаем список для проверки
        data = [1, 3, 2]
        # Желаемый результат
        expected = [3, 2, 1]
        self.assertEqual(self.program.sorted_numbers(data, "По убыванию"), expected)


# Функция для запуска всех тестов в коде написанных с помощью модуля unittest
def run_tests():
    unittest.main(argv=['first-arg-is-ignored'], exit=False)


# Запуск кода
if __name__ == "__main__":
    # Запускаем тестирование
    run_tests()
    # Создаем экземпляр класса
    app = Program()
    # Запускаем демонстрацию окна
    app.show_window()
