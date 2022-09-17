# Analisis de Datos

# Importar librerias

    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt

# Importar data

    df = pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Ejercicio-tratamiento data.xlsx")
    df1=pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Ejercicio-tratamiento data.xlsx", sheet_name='Base de datos') 
    df2=pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Ejercicio-tratamiento data.xlsx", sheet_name='indicativos')

# Verificar si se repite

    pd.crosstab(df1["Beneficio Ofrecido"], columns= "Beneficio 50-50").sort_values(by="Beneficio 50-50", ascending=False) ## CUANTAS VECES SE REPITEN LOS DATOS

## UNIR LAS DOS HOJAS DE XLSX

    df10 = pd.merge(df1, df2, on = ['Ciudad'], how ='inner') 

## ELIMINAR

    df10 = df10.drop(["Beneficio Ofrecido"], axis=1) 

## SEPARAR DATOS Y UNIRLOS EN EL DF

    Referencia1 = df10.Referencia.str.split("-",expand=True)[1] 

## AGREGAR AL DF COLUMNA

    df10["Referencia1"] = Referencia1 

## ELIMINAR ESPACIOS EN BLANCO INICIALES O POSTERIORES EN DF

    df10.columns = df10.columns.str.strip() 

## ELIMINAR LO QUE ESTE ENTRE ()

    df10["Telefono 2"] = df10["Telefono 2"].str.replace(r"\(.*\)","") 

## ELIMINAR LO QUE ESTE ENTRE ()

    df10["Telefono 1"] = df10["Telefono 1"].str.replace(r"\(.*\)","") 

## ELIMINAR REFERENCIA

    df10 = df10.drop(["Referencia"], axis=1) 

## ELIMINAR NAN

    df10nan = df10.dropna() 

## CAMBIAR OBJECT A FLOAT

    df10nan["Referencia1"] = df10nan["Referencia1"].astype(float, errors = "raise") 

## CONVERTIR A EXCEL

    df10nan.to_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Parcialtratamientodata.xlsx", index = None,  header=True)

## LEER EXCEL

    dffinal=pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Parcialtratamientodata.xlsx", sheet_name='Sheet1')

## RENOMBRAR

    dffinal.rename({'Telefono 1': 'Celular','Telefono 2': 'Telefonofijo'}, axis=1, inplace=True) 

## AGREGAR COLUMNAS

    dffinal['Indicativonacional']=60 

## ELIMINAN DUPLICADOS DE REFERENCIA1

    dffinal=dffinal.drop_duplicates(subset ="Referencia1")  

## SE CONVIERTE EN STR

    dffinal["Telefonofijo"] = dffinal["Telefonofijo"].apply(str).str.replace('.', ',') 

## VERIFICAR INFORMACIÓN DEL DATAFRAME

    dffinal.info()

## CONVERTIR A EXCEL SOLUCION

    dffinal.to_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/TratamientoDataFinall.xlsx", index = None,  header=True)

## LEER EXCEL FINAL

    Solucion=pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/TratamientoDataFinall.xlsx", sheet_name='Sheet1')

## FILTRAR COLUMNA CELULAR

## CAMBIAR INT64 A OBJECT

    Solucion["Celular"] = Solucion["Celular"].astype(str) 

## CREAR COLUMNA LONGITUD

    Solucion["Celularlongitud"] = Solucion["Celular"].str.len() 

# CREAR COLUMNA LONGITUD

    Solucion["Telefonofijolongitud"] = Solucion["Telefonofijo"].str.len() 

# CAMBIAR DE OBJECT A INT64

    Solucion["Celular"].astype(str).astype(np.int64) 
    Solucion["Indicativo"] = Solucion["Indicativo"].astype(str)
    Solucion["Referencia1"] = Solucion["Referencia1"].astype(str)
    Solucion["Indicativonacional"] = Solucion["Indicativonacional"].astype(str)

## VERIFICAR SOLUCION

    Solucion.info()
    Solucion

## FUNCION PARA UNIR VARIAS COLUMNAS Y CREAR NUEVA

    def func(x):
        return x[5] + x[3] + x[4] 
    Solucion["Telefono fijo"] = Solucion.apply(func, axis=1) 
    Solucion

    Telefonovalido = (Solucion[Solucion.Telefonofijolongitud == 5]) 

## VALORES QUE COMIENCEN POR 3

    Celular1 = Telefonovalido.Celular.str.startswith("3") 
    Telefonovalido["Celular1"]= Celular1
    Telefonovalido.head()
    Telefonovalido.info()
    Telefonovalido


## EJERCICIO SEGMENTACIÓN DE DATOS

    data = pd.read_excel(r"C:\Users\fadg1\OneDrive\Escritorio\Analytics\Prueba\ASISTENTE DE HERRAMIENTAS EXCEL/Ejercicio-Segmentación-de-datos.xlsx", sheet_name= "Data") ## SUBIR DATA
    data.head()
    data.info()
    data.describe()

    importeormesyservicio = pd.pivot_table(data, values="Importe factura", index= ["Servicios"], columns=["Mes Factura"], aggfunc=np.sum ) ## TABLA DINAMICA, SERVICIO E IMPORTE POR MES
    importeormesyservicio

    from itertools import count


    serviciosporedad = pd.pivot_table(data, values="Edad", index= ["Servicios"], aggfunc= np.mean ) ## TABLA DINAMICA SERVICIOS POR EDAD PROMEDIO
    serviciosporedad

## TABLA DINAMICA POR LOCACLIDAD E IMPORTE DE FACTURA

    importeporlocalidad = pd.pivot_table(data, values="Importe factura", index= ["Localidad"], columns=["Mes Factura"], aggfunc= np.sum )
    importeporlocalidad.pct_change()

## DIEFRENCIA PORCENTUAL DE LOCALIDADES DEL TOTAL IMPORTE FACTURA

    importeporlocalidad = pd.pivot_table(data, values="Importe factura", index= ["Localidad"], aggfunc= np.sum )

## ORDENAR TABLA DESCENDENTE

    importeporlocalidad = importeporlocalidad.sort_values(by = "Importe factura", ascending=False) 
    importeporlocalidad["TotalImporte"] = (importeporlocalidad["Importe factura"].sum())
    importeporlocalidad["promedio"] = (pd.pivot_table(data, values="Importe factura", index= ["Localidad"], aggfunc= np.mean))
    importeporlocalidad["Diferencia"] = importeporlocalidad["TotalImporte"] - importeporlocalidad["Importe factura"]
    importeporlocalidad["Diferencia porcentual %"] = (importeporlocalidad["Diferencia"] / importeporlocalidad["promedio"])*100
    importeporlocalidad

    importeporlocalidadpromedio = pd.pivot_table(data, values="Importe factura", index= ["Localidad"], aggfunc= np.mean )

## ORDENAR TABLA DESCENDENTE

    importeporlocalidadpromedio = importeporlocalidadpromedio.sort_values(by = "Importe factura", ascending=False) 
    importeporlocalidadpromedio

## TABLA DINAMICA MAYORES 20 FACTURAS POR NOMBRE

    mayoresfacturas = pd.pivot_table(data, values="Importe factura", index= ["Nombre"], aggfunc= np.max )
    mayoresfacturasascendente = mayoresfacturas.sort_values(by = "Importe factura", ascending=False)
    mayoresfacturasascendente.head(20)

### df3eliminan ## VER EL DF
### df3eliminan["Telefono 1"] = df3eliminan["Telefono 1"].str.replace(r"\(.*\)","")
### df3eliminan["Telefono 2"] = df3eliminan["Telefono 2"].str.replace(r"\(.*\)","") ##ELIMINAR LO QUE ESTE ENTRE ()
### df3eli = df3.drop(["Referencia"], axis=1) ## ELIMINAR REFERENCIA
### df3elimi = df3eli.dropna() ## ELIMINAR NAN
### df3elimi.info()
### df['Fecha'] = pd.to_datetime(df['Fecha'], format='%Y%m%d').dt.strftime('%d/%m/%Y') cambiar tipo de fecha en DF
### df [(df['col1']> 25) & (df ['col2'] <30)] # logic y SELECCIONAR COLUMNAS CON VALORES
### df [(df['col1']> 25) | (df ​​['col2'] <30)] # lógica o SELECCIONAR COLUMNAS CON VALORES
### df [!(df['col1']> 25)] # lógica no
### items_df['Final Price'] = items_df['Actual Price'] - \ ((items_df['Discount(%)']/100) * items_df['Actual Price']) ## AGREGAR OPERACIÓN DE DOS COLUMNAS

### dffinal["Telefonofijo"] = dffinal["Telefonofijo"].apply(str).str.replace('.', ',') ## SE COnvierte .a ,
### Solucion["Celular"].astype(str).astype(np.int64) CAMBIAR DE OBJECT A INT64
### Telefono = dffinal.Celular.str.startswith("3") ## VALORES QUE COMIENCEN POR 3
### dffinal["Telefono"]= Telefono

## def func(x): UNIR VARIAS COLUMNAS EN  EL DF

    ##return x[6] + x[4] + x[5] ## s

### Solucion["Telefono fijo"] = Solucion.apply(func, axis=1) 
### Solucion["Telefono fijo"] = Solucion.apply(lambda x: x.a+x.b+x.c, axis=1) SUMAR VARIAS COLUMNAS EN  EL DF


