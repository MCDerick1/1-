# 1-Для создания приложения на PyQt, которое визуализирует движение тела, брошенного под углом, мы будем использовать библиотеки PyQt5 для создания графического интерфейса и matplotlib для построения графиков. 
import sys
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.backends.backend_qt5agg import FigureCanvasQTAgg as FigureCanvas
from PyQt5.QtWidgets import QApplication, QWidget, QVBoxLayout, QLabel, QLineEdit, QPushButton

class TrajectoryPlotter(QWidget):
    def __init__(self):
        super().__init__()

        self.setWindowTitle('Визуализация движения тела под углом')
        self.setGeometry(100, 100, 800, 600)

        layout = QVBoxLayout()

        # Поле ввода для начальной скорости
        self.initial_speed_label = QLabel('Начальная скорость (в м/с):')
        self.initial_speed_input = QLineEdit()
        layout.addWidget(self.initial_speed_label)
        layout.addWidget(self.initial_speed_input)

        # Поле ввода для угла броска
        self.angle_label = QLabel('Угол броска (в градусах):')
        self.angle_input = QLineEdit()
        layout.addWidget(self.angle_label)
        layout.addWidget(self.angle_input)

        # Кнопка построить траекторию
        self.plot_button = QPushButton('Построить траекторию')
        self.plot_button.clicked.connect(self.plot_trajectory)
        layout.addWidget(self.plot_button)

        # Виджет для отображения графика
        self.canvas = FigureCanvas(plt.Figure())
        layout.addWidget(self.canvas)

        self.setLayout(layout)

    def plot_trajectory(self):
        # Считывание данных с полей ввода
        try:
            initial_speed = float(self.initial_speed_input.text())
            angle = float(self.angle_input.text())
        except ValueError:
            return
        
        # Преобразование градусов в радианы
        angle_rad = np.radians(angle)

        # Параметры движения
        g = 9.81  # Ускорение свободного падения (м/с²)
        time_of_flight = 2 * initial_speed * np.sin(angle_rad) / g
        t = np.linspace(0, time_of_flight, num=100)  # время от 0 до времени полета

        # Расчет траектории
        x = initial_speed * np.cos(angle_rad) * t
        y = initial_speed * np.sin(angle_rad) * t - 0.5 * g * t*2

        # Построение графика
        self.canvas.figure.clear()
        ax = self.canvas.figure.add_subplot(111)
        ax.plot(x, y)
        ax.set_xlabel('X -- расстояние (м)')
        ax.set_ylabel('Y -- высота (м)')
        ax.set_title('Траектория полета тела под углом')
        ax.grid()

        self.canvas.draw()

if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = TrajectoryPlotter()
    window.show()
    sys.exit(app.exec_())

-- Описание работы программы
1. Программа создает графический интерфейс с полями ввода для начальной скорости и угла броска.
2. После нажатия на кнопку "Построить траекторию", программа вычисляет траекторию полета тела.
3. График отображается с подписями осей "X -- расстояние (м)" и "Y -- высота (м)".
