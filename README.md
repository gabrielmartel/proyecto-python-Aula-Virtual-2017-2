# proyecto-python-musica-2017-2
# from tkinter import *
import matplotlib.pyplot as plt
import numpy as np
def programa():
    def runge_kutta(x, t, dt, func):
        k1 = func(x, t) * dt
        k2 = func(x + 0.5 * k1, t + 0.5 * dt) * dt
        k3 = func(x + 0.5 * k2, t + 0.5 * dt) * dt
        k4 = func(x + k3, t + dt) * dt
        x_sig = x + 1 / 6.0 * (k1 + 2 * k2 + 2 * k3 + k4)

        return x_sig

    N = int(listaP[0])
    tau = int(listaT[0])
    dt = tau / float(N - 1)
    x0 = int(listaX[0])
    v0 = int(listaV[0])
    gravedad = 9.8
    time = np.linspace(0, tau, N, endpoint=True)
    x = np.zeros([N, 2])
    x[0, 0] = x0
    x[0, 1] = v0

    def Ecu(state, time):
        g0 = state[1]
        g1 = gravedad
        return np.array([g1, g0])

    for j in range(N - 1):
        x[j + 1] = runge_kutta(x[j], time[j], dt, Ecu)

    datos_x = [x[j, 0] for j in range(N)]
    datos_v = [x[j, 1] for j in range(N)]

    plt.subplot(2, 1, 1)
    plt.plot(time, datos_x, 'ro--')
    plt.xlabel('tiempo (s)')
    plt.ylabel('posición (m)')
    plt.grid()
    plt.subplot(2, 1, 2)
    plt.plot(time, datos_v, 'bo--')
    plt.xlabel('tiempo (s)')
    plt.ylabel('velocidad (m/s)')
    plt.grid()
    plt.show()




ventana = Tk()
ventana.title("Aula Virtual")
ventana.geometry("470x500")
ventana.configure(background='spring green')
Ver = Label(ventana, text="Graficas: ", bg="red", fg="white", font=("Graficas: ", 15)).place(x=200, y=20)
separador = Label(ventana, text="\n" * 32, bg="dark violet").place(x=150, y=0)
numero = StringVar()
numero.set("Ingresa datos: ")
etiqueta1 = Label(ventana, textvariable=numero, fg="white", bg="gray29", font=(40)).place(x=5, y=20)
Puntos = StringVar()
Puntoscaja = Entry(ventana, textvariable=Puntos, width=6, font=("w", 20)).place(x=50, y=50)
Tiempo = StringVar()
Tiempocaja = Entry(ventana, textvariable=Tiempo, width=6, font=("w", 20)).place(x=50, y=100)
x0 = StringVar()
x0caja = Entry(ventana, textvariable=x0, width=6, font=("w", 20)).place(x=50, y=150)
v0 = StringVar()
v0caja = Entry(ventana, textvariable=v0, width=6, font=("w", 20)).place(x=50, y=200)

listaP = []
listaT = []
listaX = []
listaV = []


def obtener():
    listaP.append(Puntos.get())
    listaT.append(Tiempo.get())
    listaX.append(x0.get())
    listaV.append(v0.get())
    programa()


OBTENER = Button(ventana, text="Graficar", command=obtener).place(x=0,y=0)

mainloop()
