# Archivo de formato GW-01
Define el formato del archivo que se utiliza para alimentar la información del [libro de invitados](Terminology.md#guestbook).

## Registro de encabezado (requerido)
`0000000000|GW01|YYYYMMDD|CAMPAIGNID|PROCESS-TYPE|TRY-FIRST`

| Campo        | Tipo           | Tipo  |
| :----------: |-------------| -----|
| 0000000000      | string justificado | Número de registros de detalle que contiene el archivo, justificado a la izquierda con ceros hasta completar 10 posiciones. |
| GW01      | constante      |   Literal que identifica el tipo de archivo a procesar. Valor fijo: GW01 |
| YYYYMMDD | string      | Fecha en formato YYYYMMDD de la generación del archivo |
| CAMPAIGNID | string      | Valor sumistrado por Evertec. Corresponde con el identificador de la campaña en el sistema. Utilice el literal AUTO para crear una campaña para este archivo. |
| PROCESS-TYPE | string      | Valores admitidos: AUTO (enviar mensajes a clientes) NONE (no enviar mensajes a clientes)   |
| TRY-FIRST | string      | Valores admitidos: SMS (intentar enviar SMS primero) ,EMAIL (intentar enviar Email primero) |

### Ejemplo encabezado
`0000000001|GW01|20171231|729B0EA0|AUTO|SMS`

> Archivo generado el 31 de dic de 2017 para el proceso GW01. Incluye 1 registro de detalle para la campaña 729B0EA0. Intenta enviar SMS primero.

`0000000100|GW01|20171231|729B0EA0|NONE`

> Archivo generado el 31 de dic de 2017 para el proceso GW01. Incluye 100 registros de detalle para la campaña 729B0EA0. No envía mensajes.

`0000000001|GW01|20171231|AUTO|NONE`

> Archivo generado el 31 de dic de 2017 para el proceso GW01. Incluye 1 registro de detalle. La campaña se crea de forma automática. No envía mensajes.

`0000000001|GW01|20171231|729B0EA0|NONE|EMAIL`

> Registro invalido: `PROCESS-TYPE = NONE` no admite `TRY-FIRST`


## Registro(s) de detalle (requerido)

| Campo        | Tipo           | Tipo  |
| ------------- |-------------| ----- |
| Nickname      | string | Identificación del cliente en la institución |
| ActivationCode | string (opcional si PROCESS-TYPE es AUTO)   |  Código de activación generado para el valor en el campo Nickname |
| Cellphone | string (opcional)      |  Número de celular a donde se envia el SMS con el código de activación (cuando aplica) |
| EMail | string (opcional)      |  Dirección de correo electrónico a donde se envía el código de activación (cuando aplica) |

### Ejemplo detalle
`CC-123456789|123456`

> Asociar el código de activación 123456 con el cliente identificado como CC-123456789

`CC-123456789||XXXXXXXXXX`

> Si PROCESS-TYPE = AUTO y TRY-FIRST = SMS, auto-generar un código de activación, asociarlo con la identificación CC-123456789 y enviarlo por SMS al celular XXXXXXXXXX.


`CC-123456789|||user@gmail.com`

> Si PROCESS-TYPE = AUTO y TRY-FIRST = EMAIL, auto-generar un código de activación, asociarlo con la identificación CC-123456789 y enviarlo por coreo a user@gmail.com

## Ejemplo archivo
```
0000000003|GW01|20171231|729B0EA0|AUTO|SMS
CC-123456789||+441632960675
CC-987654321|||name1@gmail.com
CC-874521369||+441632000000|name2@gmail.com
```
> Archivo con tres registros para la campaña 729B0EA0. Auto-generar código de activación y enviar como SMS (primer intento) o como Email (segundo intento).

```
0000000002|GW01|20171231|729B0EA0|NONE
CC-123456789|000000
CC-987654321|111111
```

> Archivo con dos registros para la campaña 729B0EA0. Relacionar códigos de activación (la instituación ya los envió). 
