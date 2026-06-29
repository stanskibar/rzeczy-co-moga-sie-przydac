import datetime
import sys

from PyQt5.QtCore import pyqtSlot
from PyQt5.QtWidgets import QApplication, QMainWindow, QWidget, QVBoxLayout, QTableView, QHBoxLayout, QLineEdit, \
    QPushButton, QSpinBox
from PyQt5.QtGui import QPalette, QColor

class Osoba:
    def __init__(self, imie, nazwisko, firma):
        self.imie = imie
        self.nazwisko = nazwisko
        self.firma = firma

class Wejscie:
    def __init__(self, osoba, typ, data):
        self.osoba = osoba
        self.typ = typ
        self.data = data

class EntryManager:
    def __init__(self):
        self.entries = []
        self.lista_osob_wewnatrz = []

    def rejestruj_wejscie(self, osoba):
        self.entries.append(Wejscie(osoba,"Wejscie",datetime.datetime.now()))
        self.lista_osob_wewnatrz.append(osoba)

    def rejestruj_wyjscie(self, osoba):
        for osoby in self.lista_osob_wewnatrz:
            if osoba.nazwisko == osoby.nazwisko:
                self.lista_osob_wewnatrz.remove(osoby)
                self.entries.append(Wejscie(osoba,"Wyjscie",datetime.datetime.now()))
                print("Zarejestrowano wyjscie")
            else:
                print("Brak takiej osoby")




class MainWindow(QMainWindow):

    def __init__(self):
        super().__init__()

        self.setWindowTitle("Entry Manager")
        glownylayout = QVBoxLayout()
        glownyWidget = QWidget()
        glownyWidget.setLayout(glownylayout)

        gorny_layout = QHBoxLayout()
        gorny_widget = QWidget()
        gorny_widget.setLayout(gorny_layout)

        glownylayout.addWidget(gorny_widget)
        self.tabela = QTableView()
        glownylayout.addWidget(self.tabela)

        przyciski_dodawanie_layout = QVBoxLayout()
        filtrowanie_layout = QVBoxLayout()
        wykresy_layout = QVBoxLayout()

        #przyciski
        self.imie = QLineEdit()
        self.nazwisko = QLineEdit()
        self.firma = QLineEdit()
        self.rejestruj_wejscie = QPushButton("Wejscie")
        self.rejestruj_wyjscie = QPushButton("Wyjscie")

        przyciski_dodawanie_layout.addWidget(self.imie)
        przyciski_dodawanie_layout.addWidget(self.nazwisko)
        przyciski_dodawanie_layout.addWidget(self.firma)
        przyciski_dodawanie_layout.addWidget(self.rejestruj_wejscie)
        przyciski_dodawanie_layout.addWidget(self.rejestruj_wyjscie)

        przyciski_widget = QWidget()
        przyciski_widget.setLayout(przyciski_dodawanie_layout)
        gorny_layout.addWidget(przyciski_widget)


        self.nazwisko_filtr = QLineEdit()
        self.firma_filtr = QLineEdit()
        self.czas_minuty = QSpinBox()
        filtrowanie_layout.addWidget(self.nazwisko_filtr)
        filtrowanie_layout.addWidget(self.firma_filtr)
        filtrowanie_layout.addWidget(self.czas_minuty)
        filtrowanie_widget = QWidget()
        filtrowanie_widget.setLayout(filtrowanie_layout)
        gorny_layout.addWidget(filtrowanie_widget)


        self.setCentralWidget(glownyWidget)


        self.manager = EntryManager()
        self.rejestruj_wejscie.clicked.connect(self.rejestruj_wejscie_fun)
        self.rejestruj_wyjscie.clicked.connect(self.rejestruj_wyjscie_fun)

    def rejestruj_wejscie_fun(self):
        osoba = Osoba(self.imie.text(), self.nazwisko.text(), self.firma.text())
        self.manager.rejestruj_wejscie(osoba)
        print("Zawiscie")

    def rejestruj_wyjscie_fun(self):
        osoba = Osoba(self.imie.text(), self.nazwisko.text(), self.firma.text())
        self.manager.rejestruj_wyjscie(osoba)
        print("Zawiscie")


# Press the green button in the gutter to run the script.
if __name__ == '__main__':
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    app.exec()
