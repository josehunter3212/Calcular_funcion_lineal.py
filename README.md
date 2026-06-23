# Calcular_funcion_lineal.py
import os
import sys
import matplotlib.pyplot as plt

# Importación de la biblioteca Qt requerida (PyQt5)
from PyQt5.QtWidgets import QApplication, QMessageBox

# --- CONSTANTES ---
OPCION_LINEAL_INDIVIDUAL = "1"
OPCION_REPETIR_DOS = "2"
OPCION_REPETIR_TRES = "3"
OPCION_REPETIR_N = "4"
OPCION_SALIR = "5"


# --- FUNCIONES ---

def calcular_funcion_lineal():
    """
    Pide los valores al usuario, calcula la función lineal y despliega la gráfica.
    """
    print("\n--- [EJECUCIÓN] Cálculo de Función Lineal ---")
    try:
        # 1. Pedir parámetros al usuario
        m = float(input("👉 Ingrese la pendiente (m): "))
        b = float(input("👉 Ingrese la ordenada al origen (b): "))
        valor_x = float(input("👉 Ingrese el valor de x a evaluar: "))

        # 2. Calcular punto específico
        resultado = m * valor_x + b
        print(f"\n✨ [RESULTADO]: Cuando x = {valor_x}, f(x) = {resultado}")

        # 3. Crear el rango para la gráfica utilizando listas por comprensión
        x_lista = [-10 + (i * 20 / 99) for i in range(100)]
        y_lista = [m * x + b for x in x_lista]

        # 4. Graficar usando matplotlib
        plt.figure(figsize=(6, 4))
        plt.plot(x_lista, y_lista, label=f'f(x) = {m}x + {b}', color='blue')
        plt.scatter(valor_x, resultado, color='red', zorder=5, label=f'Punto ({valor_x}, {resultado})')

        plt.title('Gráfica de una Función Lineal')
        plt.xlabel('Eje X')
        plt.ylabel('Eje Y')
        plt.axhline(0, color='black', linewidth=0.8)
        plt.axvline(0, color='black', linewidth=0.8)
        plt.grid(color='gray', linestyle='--', linewidth=0.5)
        plt.legend()
        print("📊 Mostrando gráfica... Cierre la ventana de la imagen para continuar.")
        plt.show()

    except ValueError:
        print("❌ Error: Por favor, ingrese números válidos.")


def repetir_funcion(funcion_a_ejecutar, veces, total_veces):
    """
    Función de orden superior. Ejecuta 'funcion_a_ejecutar' las veces indicadas.
    RESTRICCIÓN CUMPLIDA: No utiliza bucles (ni for ni while), se basa en recursión pura.
    """
    if veces <= 0:
        print("\n✅ ¡Se han completado todas las ejecuciones de este bloque!")
        return

    # Calcular matemáticamente el número de ejecución actual
    ejecucion_actual = total_veces - veces + 1
    print(f"\n{"=" * 40}")
    print(f"🔄 EJECUTANDO FUNCIÓN POR {ejecucion_actual}ª VEZ (De un total de {total_veces})")
    print(f"{"=" * 40}")

    # Ejecución de la función de orden superior
    funcion_a_ejecutar()

    # Llamada recursiva (reemplaza al bucle)
    repetir_funcion(funcion_a_ejecutar, veces - 1, total_veces)


def mostrar_menu():
    """Muestra la interfaz de usuario basada en texto (TUI) con múltiples opciones"""
    print("\n==================================================")
    print("           SISTEMA DE MÚLTIPLES OPCIONES          ")
    print("==================================================")
    print(f" [{OPCION_LINEAL_INDIVIDUAL}] Calcular función lineal una sola vez")
    print(f" [{OPCION_REPETIR_DOS}] Ejecutar 'Repetir Función' (2 veces consecutivas)")
    print(f" [{OPCION_REPETIR_TRES}] Ejecutar 'Repetir Función' (3 veces consecutivas)")
    print(f" [{OPCION_REPETIR_N}] Definir manualmente cuántas veces repetir")
    print(f" [{OPCION_SALIR}] Salir del programa")
    print("==================================================")


def ejecutar_programa():
    """Controla el flujo principal y la persistencia de la TUI"""
    # Inicialización requerida para usar componentes de Qt
    app = QApplication(sys.argv)

    continuar = True
    while continuar:
        mostrar_menu()
        opcion = input("Seleccione un número de opción: ").strip()

        if opcion == OPCION_LINEAL_INDIVIDUAL:
            print("\n🚀 Iniciando ejecución única...")
            calcular_funcion_lineal()

        elif opcion == OPCION_REPETIR_DOS:
            print("\n🚀 Iniciando bloque de 2 repeticiones...")
            # Pasa la función como argumento y el número 2
            repetir_funcion(calcular_funcion_lineal, 2, 2)

        elif opcion == OPCION_REPETIR_TRES:
            print("\n🚀 Iniciando bloque de 3 repeticiones...")
            # Pasa la función como argumento y el número 3
            repetir_funcion(calcular_funcion_lineal, 3, 3)

        elif opcion == OPCION_REPETIR_N:
            try:
                n = int(input("\n🔢 ¿Cuántas veces deseas repetir la función?: "))
                if n > 0:
                    print(f"\n🚀 Iniciando bloque personalizado de {n} repeticiones...")
                    repetir_funcion(calcular_funcion_lineal, n, n)
                else:
                    print("❌ Debe ingresar un número mayor a 0.")
            except ValueError:
                print("❌ Entrada inválida. Debe ser un número entero.")

        elif opcion == OPCION_SALIR:
            print("\n👋 Saliendo del programa. ¡Hasta luego!")

            # Cuadro de diálogo nativo de Qt al salir
            msg = QMessageBox()
            msg.setIcon(QMessageBox.Information)
            msg.setText("Programa Finalizado con éxito.")
            msg.setWindowTitle("Salida")
            msg.exec_()

            continuar = False
        else:
            print("\n❌ Opción no válida. Por favor, digite un número del 1 al 5.")


# --- BLOQUE PRINCIPAL ---
if __name__ == "__main__":
    # Limpieza de pantalla inicial utilizando la biblioteca 'os'
    os.system('cls' if os.name == 'nt' else 'clear')
    print("🤖 Sistema Multiopciones Iniciado.")
    ejecutar_programa()
