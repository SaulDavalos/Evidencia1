# Evidencia1
Primera evidencia del semestre 
import datetime
from collections import namedtuple

Registro = namedtuple("Registro", ("fecha", "nombre", "monto", "servicio")) #Tupla nominada que le dara un orden a los datos que guardamos
diccionarioRegistros = {} #Diccionario que gauradara todos los registros donde la llave sea el folio
servicio = {} #Un diccionario que guardara los servicios con su detalle
monto = 0
lista = []
fechaRegistros = {} #Diccionario que guardara los registros donde la llave sea la fecha

while True:
    print("***Menu principal***")
    print("1-Registrar un servicio")
    print("2-Consultar un servicio")
    print("3-Consultar una fecha")
    print("4-Ver todos los registros")
    print("5-Apagar")
    opcionMenu = int(input("Dime la opcion: "))

    if opcionMenu == 1:
        fecha_capturada = input("Dame una fecha de tu servicio con el siguiente formato dd/mm/yyyy: ")
        fecha = datetime.datetime.strptime(fecha_capturada, "%d/%m/%Y").date()
        nombre = input("Dame tu nombre: ")
        if diccionarioRegistros.keys():
            folio = max(diccionarioRegistros.keys()) + 1
        else:
            folio = 1

        while True:
            articulo = input("Dime el articulo al que le haremos el servicio: ")
            if articulo == "":
                print("No me has dado nada de tu articulo")

            else:
                descripcion_servicio = input("Dime el servicio que necesitara tu articulo: ")
                precio = float(input("Dime el precio de tu servicio: "))
                monto = precio + monto
                servicio[articulo] = descripcion_servicio

            seguir = int(input("Si quieres agregar otro articulo pon 0, si quieres salir 1: "))
            if seguir == 1:
                break

        print(f"Lo que pagaras por tus servicios sera {monto}")
        IVA_monto = monto * .16
        monto_IVA = monto + IVA_monto
        print(f"El monto de lo que pagara con IVA es {monto_IVA}")
        registro = Registro(fecha, nombre, monto, servicio)
        diccionarioRegistros[folio] = registro
        fechaRegistros[fecha] = registro
        lista.append(fecha)
        monto = 0
        servicio = {}

    if opcionMenu == 2:
        llave = int(input("Dame el folio del que quieres buscar: "))
        print(f"Nombre: {diccionarioRegistros[llave].nombre}")
        print(f"Fecha: {diccionarioRegistros[llave].fecha}")
        print(f"Monto: {diccionarioRegistros[llave].monto}")
        print(f"Servicio: {diccionarioRegistros[llave].servicio}")

    if opcionMenu == 3:
        fecha_ = input("Dime una fecha para buscar: ")
        fecha_buscar = datetime.datetime.strptime(fecha_, "%d/%m/%Y").date()
        print(f"Fecha del ticket: {fecha_buscar}\n")
        print(f'{"Fecha":<5} | {"Nombre":<10} | {"Monto":<15} | {"Dispositivo/Servicio":<20} \n')
        for elemento in fechaRegistros[fecha]:
            while lista:
                if fecha_buscar == lista[0]:
                    print(f"{fechaRegistros[elemento].fecha} | {fechaRegistros[elemento].nombre} | {fechaRegistros[elemento].monto} | {fechaRegistros[elemento].servicio}")
                lista.pop(0)

    if opcionMenu == 4:
        print("Folio\tNombre\tFecha\tMonto\tDescripcion")
        print("-"*100)
        for folio in diccionarioRegistros:
            print(f"{folio}\t{diccionarioRegistros[folio].nombre}\t{diccionarioRegistros[folio].fecha}\t{diccionarioRegistros[folio].monto}\t{diccionarioRegistros[folio].servicio}")

    if opcionMenu == 5:
        break
