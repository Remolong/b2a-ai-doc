# Web Service Risk Balances

Puede probar los servicios web con [Postman](https://www.postman.com/).

Para obtener más información, consulte Postman. [Documentation](POSTMAN.md).

### Descripciones de los parámetros

| Parametro | Tipo |  Descripcion |
| ---------- | --- | --- |
| ws_code | String | Su autenticación `ws_code`. |
| ws_secret_key | String |  Su autenticación `ws_secret_key`. |
| users | Array | Esta es una lista de clientes para actualizar. Cada cliente para actualizar debe contener el parámetro `customer_cif` y` risk_balance`. |
| customer_cif | String | La identificación del usuario / cliente. |
| risk_balance | Float | El balance de riesgos. |

## Descripcion del Servicio Web

- URL del servicio web

```
    your-domain.tdl/wp-json/api/v1/risk-balances
```

| METODO | Endpoint | Parametros |  Descripcion |
| ------ | -------- | ---------- | --- |
| **POST** | /update |  `ws_code`, `ws_secret_ket`, `users` |  Este enpoint permite la actualizacion de los balances de riesgos de los usuarios. |

### Este es un ejemplo de actualización de balances de riesgos de los usuarios.

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