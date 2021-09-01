# Sistema B2A Eisen
## Documentación técnica

La plataforma dispone de mecanismos mediante los cuales interactuar con el sistema en determinados procesos.

Estos mecanismos permiten a los desarrolladores y técnicos del **Asociado Industrial (AI)** establecer procesos automatizados para la comunicación de determinados datos entre la plataforma **B2A** y los sistemas propios del AI, así como entre los sistemas del AI y B2A.

Las opciones de interacción disponibles son:

* [Comunicación de pedidos](#comunicación-de-saldos-de-riesgo-de-los-clientes)
* [Comunicación de saldos de riesgo de los clientes](#comunicación-de-saldos-de-riesgo-de-los-clientes)
  * [Comunicación de saldos de riesgo mediante Servicio web](#servicio-web-para-la-comunicación-de-saldos-de-riesgo)  
  * [Comunicación de saldos de riesgo mediante SFTP](#servicio-sftp-para-la-comunicación-de-saldos-de-riesgo)

***
## Comunicación de pedidos

**B2A** guarda cada uno de los pedidos que el **AI** o sus **clientes** realizan mediante la plataforma.
Cada uno de estos pedidos se almacena en un archivo, y el formato del mismo será el definido por el AI en el momento de la implantación. 
* Formatos disponibles
    * JSON (Recomendado)
    * XML
    * CSV

Los archivos se almacenan en la carpeta "**pedidos**" del [espacio SFTP](#espacio-sftp-del-ai) puesto a disposición del AI. En esta carpeta 
La operativa habitual podría ser descargar cada archivo y eliminarlo una vez tratado, de ese modo en esta carpeta siempre estarán los archivos correspondientes a pedidos pendientes de gestionar por parte del AI. 

B2A generará el archivo en el momento en que el usuario confirma el pedido, y también cuando el mismo va cambiando de estado. 
En el contenido del archivo se puede ver el estado, junto al resto de datos del pedido.
***

## Comunicación de saldos de riesgo de los clientes

Esta comunicación permite al AI informar a B2A el saldo de riesgo disponible en cada momento para cada uno de sus clientes, 
de manera que este pueda controlar en el momento en que el cliente intenta realizar un pedido, si dispone de riesgo suficiente, 
y en caso contrario avisarle con el mensaje correspondiente.

Esta comunicación se puede realizar de dos maneras diferentes:
* Vía [servicio web](#servicio-web-para-la-comunicación-de-saldos-de-riesgo)
* Vía [SFTP](#servicio-sftp-para-la-comunicación-de-saldos-de-riesgo)

***
### Servicio Web para la comunicación de saldos de riesgo
***

Mediante este sistema la actualización del saldo de riesgo de los clientes es inmediata. Se puede lanzar el servicio web para actualizar el saldo de un solo cliente 
o de un grupo (puede ser de todos).

Se pueden probar los servicios web con [Postman](https://www.postman.com/).

Para ello puedes consultar nuestra [documentación](POSTMAN.md).

### Descripción de los parámetros

| Parametro | Tipo |  Descripcion |
| ---------- | --- | --- |
| ws_code | String | Su autenticación `ws_code`. |
| ws_secret_key | String |  Su autenticación `ws_secret_key`. |
| users | Array | Esta es una lista de clientes para actualizar. Cada cliente para actualizar debe contener los datos `customer_cif` y` risk_balance`. |
| customer_cif | String | La identificación del usuario / cliente. |
| risk_balance | Float | El saldo de riesgo del cliente. |

### Descripcion del Servicio

- URL del servicio web

```
    your-domain.tdl/wp-json/api/v1/risk-balances
```

| METODO | Endpoint | Parametros |  Descripcion |
| ------ | -------- | ---------- | --- |
| **POST** | /update |  `ws_code`, `ws_secret_ket`, `users` |  Este endpoint permite la actualizacion de los saldos de riesgo de los clientes. |

Los datos necesarios para lanzar el servicio web que son específicos del AI, 
se pueden obtener en el apartado Administración/Ajustes del sistema de la tienda, accediendo con un usuario del AI. 

Estos datos son:
* ws_code: Código WS
* ws_secret_key: Clave secreta WS
* URL: URL del endpoint

#### Este es un ejemplo de actualización de saldos de riesgo de los clientes.

- URL:        ` your-domain.tdl/wp-json/api/v1/risk-balances/update `

- METODO:     ` POST `

- PARAMETROS

```
    {
                "ws_code":"ai27",
                "ws_secret_key":"123456789!",
                "users":
                [
                    {
                        "customer_cif":"A01010101",
                        "risk_balance":9621.43
                    },
                    {
                        "customer_cif":"B02020202",
                        "risk_balance":6805.2
                    },
                    {
                        "customer_cif":"C03030303",
                        "risk_balance":5000
                    }
                ]
            }        
```

### Respuestas

- Ok

```
{
    "status": 1,
    "message": "Ok"    
}
```

- Error

```
{
    "status": 0,
    "message": "Mensaje de error de la API."    
}
```

***
### Servicio SFTP para la comunicación de saldos de riesgo
***
Este sistema de actualización del saldo de riesgo de los clientes no es inmediato, 
ya que B2A lo gestiona mediante barridos por lotes, por ello la actualización puede demorarse hasta 5 minutos.

Al igual que en el servicio web, se puede actualizar el saldo de un solo cliente o de un grupo (puede ser de todos).

El sistema consiste en depositar archivos en la carpeta "**riesgos**" del [espacio SFTP](#espacio-sftp-del-ai) del AI, 
estos archivos serán procesados por B2A, y eliminados para no volver a procesarlos. 
El archivo a depositar en la carpeta "**riesgos**" debe contener la información en formato **JSON**. Una vez depositado el archivo en el SFTP, B2A lo gestionará en el siguiente barrido y eliminará el archivo.

#### Estructura del JSON

Lo siguiente es un ejemplo donde se puede ver la estructura que debe tener el JSON contenido en el archivo.
```
    {
        "ws_code":"ai27",
        "ws_secret_key":"123456789!",
        "data":
        [
            {
                "customer_cif":"A01010101",
                "risk_balance":9621.43
            },
            {
                "customer_cif":"B02020202",
                "risk_balance":6805.2
            },
            {
                "customer_cif":"C03030303",
                "risk_balance":5000
            }
        ]
    }        
```

#### Espacio SFTP del AI

Los AI's tienen acceso a un espacio en el servidor SFTP, dentro del cual existen carpetas para cada tipo de comunicación.

Se debe utilizar el protocolo SFTP, y se pueden obtener los datos de acceso accediendo a **Administración/Ajustes del sistema** en la tienda del AI, con un usuario del propio AI, 
 en el apartado Credenciales SFTP.
* Host
* Puerto
* Nombre de usuario
* Contraseña