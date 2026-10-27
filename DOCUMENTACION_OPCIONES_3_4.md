# Opciones 3 y 4 del formulario CONAFOVICER

## Propósito

Las opciones nuevas permiten actualizar datos personales, domiciliarios, laborales y familiares dentro de una misma campaña. La opción 3 presenta las cuatro secciones como pasos consecutivos; la opción 4 mantiene todas las secciones en una página con desplazamiento vertical. Comparten estilos, reglas de validación, catálogo UBIGEO y serialización del modelo.

## Campos incluidos

| Sección | Campos | Reglas |
|---|---|---|
| Identificación y contacto | Tipo y número de documento, nombres, apellidos, fecha de nacimiento, celular, correo | Todo obligatorio salvo correo. DNI: 8 dígitos; carné de extranjería, pasaporte y otros: patrón alfanumérico apropiado al tipo. Fecha válida y no futura. Celular móvil peruano: 9 dígitos iniciado en 9. Correo validado si se completa. |
| Domicilio | Dirección, departamento, provincia, distrito y UBIGEO | Todos obligatorios. Los selectores dependientes evitan combinar ubicaciones de distintas jurisdicciones. La dirección acepta formatos libres. |
| Información laboral | Categoría, especialidad, situación de trabajo en obra, RUC o nombre del empleador, proyecto u obra | Categorías: Peón, Oficial y Operario. Especialidad obligatoria como texto libre con sugerencias abiertas. Si trabaja en obra, debe declarar el RUC de 11 dígitos o marcar que lo desconoce y completar el nombre de la empresa; el proyecto/obra sigue siendo obligatorio. Si no trabaja, se serializan nulos para empleador y obra. |
| Familia y autorización | Cantidad de hijos menores, edad y sexo de cada hijo, consentimiento | Cantidad desde cero; edad entera de 0 a 17 y sexo obligatorio para cada hijo. No se solicitan nombres ni documentos de los menores. Consentimiento obligatorio. |

La fecha se captura con el control nativo `input type=date`, que deja al navegador/dispositivo presentar su selector táctil de año, mes y día y permite escribirla según la plataforma. El valor interno se conserva como `AAAA-MM-DD`; las comparaciones no convierten la fecha a medianoche UTC. El navegador rechaza fechas de calendario imposibles y el límite máximo es la fecha local actual.

## Presentación y carrusel

Las opciones 3 y 4 reutilizan el carrusel y el orden de piezas de la opción 2: `banner-01` y `banner-02`, con sus variantes móviles. Incluye avance automático cada cinco segundos, botones anterior/siguiente, indicadores, teclado, gesto táctil, pausa al enfocar o pasar el puntero y una alternativa visible si falla una imagen. Respeta la preferencia de movimiento reducido.

Ambas páginas muestran los textos de campaña acordados: «REGÍSTRATE Y PARTICIPA», la descripción sobre contacto y beneficios de CONA, el mensaje del sorteo de electrodomésticos y «* Campos obligatorios.». La sección Domicilio usa dirección de una línea que crece al escribir y presenta Departamento, Provincia y Distrito en tres columnas en escritorio y una columna en celular. En Opción 3, los títulos de Domicilio e Información laboral no repiten la numeración del indicador de pasos.

En Información laboral, Categoría y Especialidad se alinean en la primera fila. La situación laboral se selecciona con radios visibles y accesibles. Al responder Sí aparecen los datos del empleador y la obra; al responder No se ocultan y dejan de ser obligatorios. Si se desconoce el RUC, se ofrece el campo obligatorio para el nombre de la empresa. Los campos mantienen sus valores al alternar las respuestas, pero el envío de demostración serializa únicamente los campos correspondientes a la respuesta final.

## Diferencias frente a las propuestas originales

Se mantienen intactos los archivos `opcion-1.html` y `opcion-2.html`. Las opciones 3 y 4 agregan nombres y apellidos separados, tipo de documento, nacimiento, domicilio detallado, especialidad, pregunta condicional sobre obra, RUC/proyecto y un modelo de hijos con edad y sexo. El número de hijos cero está soportado. Se conserva la categoría laboral, celular, correo, selectores UBIGEO encadenados, control dinámico de hijos, lectura del consentimiento y botón «Actualizar datos y participar».

## Modelo preparado para integración

La validación arma en memoria el objeto siguiente, con la misma forma en ambas opciones y distinto `opcion_formulario`:

```json
{
  "tipo_documento": "DNI",
  "numero_documento": "00000000",
  "nombres": "",
  "apellidos": "",
  "fecha_nacimiento": "AAAA-MM-DD",
  "celular": "",
  "correo": null,
  "direccion": "",
  "departamento": "",
  "provincia": "",
  "distrito": "",
  "ubigeo": "",
  "actualmente_trabajando": false,
  "categoria": "",
  "especialidad": "",
  "ruc_empleador": null,
  "nombre_empleador": null,
  "no_conoce_ruc_empleador": false,
  "proyecto_obra": null,
  "hijos_menores": 0,
  "hijos": [],
  "consentimiento_datos_personales": true,
  "version_consentimiento": "2026-10-27",
  "fecha_registro": "ISO-8601",
  "opcion_formulario": "opcion-3"
}
```

Los campos existentes `categoria`, `celular`, `correo`, `ubigeo`, `hijos_menores` y `consentimiento_datos_personales` mantienen nombres compatibles; el arreglo `hijos` reemplaza los datos sueltos por una representación estructurada. No se persiste el objeto ni se emite a consola, URL o servicio externo.

## Demostración y pendientes productivos

El modo actual valida y muestra de forma explícita que los datos no se guardaron ni enviaron. No hay endpoint, base de datos, autenticación ni OTP configurados. La carga UBIGEO usa un catálogo remoto principal y fuentes de contingencia, por lo que requiere conexión de red.

El consentimiento está versionado y ahora menciona fecha de nacimiento, domicilio, información laboral y edad/sexo de hijos menores. El texto es una propuesta de demostración: el área legal debe revisar finalidades, base jurídica, destinatarios, conservación, canales de ejercicio de derechos y vigencia normativa antes de producción. La implementación no incorpora una autorización separada para fines distintos.

Antes de integrar almacenamiento, definir y documentar el endpoint, controles de seguridad, acceso y retención; evaluar OTP/autenticación; acordar el contrato definitivo de datos; y hacer revisión legal del consentimiento. No se presentan esas integraciones como implementadas.

## Archivos

- `opcion-3.html`: interfaz por pasos.
- `opcion-4.html`: página única por secciones.
- `assets/formulario-ampliado.css` y `assets/formulario-ampliado.js`: recursos comunes.
- `assets/conafovicer-logo.png`: logotipo reutilizado de la propuesta existente.

Las opciones 3 y 4 llaman el mismo inicializador del carrusel, validaciones y serialización. La única diferencia de presentación es la navegación: cuatro pasos frente a desplazamiento de página única.
