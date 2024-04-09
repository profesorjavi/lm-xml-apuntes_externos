# Esquemas XML

## Introducción

XML Schema es un lenguaje de esquema utilizado para describir la
estructura y las restricciones de los contenidos de los documentos XML
de una forma muy precisa, más allá de las normas sintácticas impuestas
por el propio lenguaje XML. Se consigue así una percepción del tipo de
documento con un nivel alto de abstracción. Fue desarrollado por el
World Wide Web Consortium (W3C) y alcanzó el nivel de recomendación en
mayo de 2001.

XML Schema está pensado para proporcionar una mayor potencia expresiva
que las DTD, menos capaces al describir los documentos a nivel formal.

Las ventajas que aporta son:

-   Se basa en XML, por lo tanto tiene todas las ventajas de un
    documento XML.
-   Tiene una gran variedad de tipos de datos.
-   Tiene un modelo de datos abierto.
-   Soporta espacios de nombres (singularidad de los nombres, algo que
    no podíamos tener con los DTDs).
-   Soporta tipos en los atributos.

## Nodo raíz `schema`

Los esquemas mantienen la estructura y la sintaxis de los documentos XML
por lo tanto deben tener un nodo raíz y dentro de él contener todo el
esquema. Por supuesto debe tener el prólogo (es decir, la definición de
la versión XML).

    <?xml version="1.0" encoding="UTF-8"?>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
      ...
    </xs:schema>

Fíjese en que hace referencia a una URL, esa dirección contiene la
definición de todos los elementos y atributos que se pueden utilizar en
un esquema. Eso no quiere decir que para programar en XML se necesite
estar conectado a Internet.

Utiliza un atributo `xmlns` que significa *XML namespace* para crear un
espacio de nombres de XML vinculado a un prefijo que en este caso es
`xs`, pero que puede ser cualquier cosa ya que sólo hace la función de
variable (también se usa `xsd`). Sirve para que cada vez que se utilice
un elemento o tipo de esa “librería” se le ponga ese prefijo, en el caso
de que se utilizasen más referencias a otras URLs se distinguiría
claramente los elementos de cada uno de ellos por medio de los prefijos.

Utilizando este atributo `xmlns`, crea un espacio de nombres para cada
URL referenciada, así si hubiese dos elementos con el mismo nombre se
diferencian claramente.

## Declaración de elementos

### Tipos de elementos

Existen tres tipos de elementos a definir en un esquema:

Elementos estándar  
Sin hijos y sin restricciones ni atributos.

Elementos simples  
Elementos sin hijos pero con restricciones y sin atributos.

Elementos complejos  
Elementos con hijos y/o atributos.

<figure>
<img src="/imagenes/40_esquemas_xml/personal_interno.png"
class="align-center"
alt="/imagenes/40_esquemas_xml/personal_interno.png" />
<figcaption>Jerarquía de ejemplo.</figcaption>
</figure>

En este ejemplo tenemos los siguientes tipos de elementos:

-   Elementos complejos: `PERSONAL` e `INTERNO` (con hijos).
-   Elementos simples: `TELEFONO` y `DIRECCIÓN` (con restricciones).
-   Elementos estándar: `NOMBRE` (sin hijos ni restricciones).

### Estructuras

Las dos estructuras posibles son:

    <xs:element name="nombre_del_elemento" type="tipo" />

Esta estructura es vacía, es decir se define un nombre de elemento y un
tipo, el tipo puede ser un estándar de XML o un tipo definido por
nosotros que describiría aquellos elementos que tengan hijos (complejos
o aquellos elementos que no tienen hijos pero tienen restricciones).

Seguido de esta definición vendría debajo la definición del tipo. Esta
estructura es la más aconsejable pero no la única.

    <xs:element name="nombre_del_elemento">
      ...
    </xs:element>

Esta estructura sería equivalente a la anterior sólo que en este caso no
se pone el tipo del elemento como atributo sino que esa definición se
haría entre las etiquetas de inicio y cierre de `element`.

Puede contener atributos cuyos valores siempre van entre comillas:

`name`  
Nombre del elemento.

`type`  
Tipo simple predefinido, ya sean los estándares o unos propios.

`maxOccurs`  
Número máximo de veces que puede aparecer `[0..unbounded]`.

`minOccurs`  
Número mínimo de veces que puede aparecer.

`ref`  
Para importar de otros esquemas o hacer referencia a un elemento ya
declarado anteriormente en este mismo esquema.

Note

Sin poner `maxOccurs` ni `minOccurs`, este elemento aparece siempre
exactamente una sola vez.

### Tipos de datos

En las dos tablas siguientes se enumeran los tipos de datos primitivos y
derivados que podemos usar en los esquemas XML.

#### Primitivos

<table style="width:99%;">
<colgroup>
<col style="width: 51%" />
<col style="width: 47%" />
</colgroup>
<thead>
<tr class="header">
<th>Tipo de dato</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>string</code></td>
<td>Representa cadenas de caracteres.</td>
</tr>
<tr class="even">
<td><code>boolean</code></td>
<td>Representa valores booleanos, que son <code>true</code> o
<code>false</code>.</td>
</tr>
<tr class="odd">
<td><code>decimal</code></td>
<td>Representa números de precisión arbitraria.</td>
</tr>
<tr class="even">
<td><code>float</code></td>
<td>Representa números de punto flotante de 32 bits de precisión
simple.</td>
</tr>
<tr class="odd">
<td><code>double</code></td>
<td>Representa números de punto flotante de 64 bits de doble
precisión.</td>
</tr>
<tr class="even">
<td><code>duration</code></td>
<td>Representa una duración de tiempo. El modelo de
<code>duration</code> es <code>PnYnMnDTnHnMnS</code>, donde
<code>nY</code> representa el número de años, <code>nM</code> el número
de meses, <code>nD</code> el número de días, <code>T</code> el separador
de fecha y hora, <code>nH</code> el número de horas, <code>nM</code> el
número de minutos y <code>nS</code> el número de segundos.</td>
</tr>
<tr class="odd">
<td><code>dateTime</code></td>
<td>Representa una instancia específica de tiempo. El modelo de
<code>dateTime</code> es <code>CCYY-MM-DDThh:mm:ss</code> donde
<code>CC</code> representa el siglo, <code>YY</code> el año,
<code>MM</code> el mes y <code>DD</code> el día, precedido por un
carácter negativo (<code>-</code>) inicial opcional para indicar un
número negativo. Si se omite el carácter negativo, se supone positivo
(<code>+</code>). La <code>T</code> es el separador de fecha y hora, y
<code>hh</code>, <code>mm</code> y <code>ss</code> representan la hora,
minutos y segundos, respectivamente. Se pueden utilizar dígitos
adicionales para aumentar la precisión de los segundos decimales, si se
desea. Por ejemplo, se admite el formato <code>ss.ss...</code> con
cualquier número de dígitos después del separador decimal. Es opcional
la parte de segundos decimales.</td>
</tr>
<tr class="even">
<td><code>time</code></td>
<td>Representa una instancia de tiempo que se repite cada día. El modelo
de <code>time</code> es <code>hh:mm:ss.sss</code> con un indicador
opcional de zona horaria.</td>
</tr>
<tr class="odd">
<td><code>date</code></td>
<td>Representa una fecha de calendario. El modelo de <code>date</code>
es <code>CCYY-MM-DD</code> con un indicador opcional de zona horaria
como el de <code>dateTime</code>.</td>
</tr>
<tr class="even">
<td><code>hexBinary</code></td>
<td>Representa datos binarios arbitrarios codificados en hexadecimal.
<code>hexBinary</code> es el conjunto de secuencias de longitud finita
de octetos binarios. Cada octeto binario se codifica como una tupla de
caracteres que se compone de dos dígitos hexadecimales
(<code>[0-9a-fA-F]</code>) y representa el código del octeto.</td>
</tr>
<tr class="odd">
<td><code>base64Binary</code></td>
<td>Representa datos binarios arbitrarios codificados en Base64.
<code>base64Binary</code> es el conjunto de secuencias de longitud
finita de octetos binarios.</td>
</tr>
<tr class="even">
<td><code>anyURI</code></td>
<td>Representa un identificador URI como lo define RFC 2396. Un valor
<code>anyURI</code> puede ser absoluto o relativo, y puede tener un
identificador de fragmento opcional.</td>
</tr>
<tr class="odd">
<td><code>QName</code></td>
<td>Representa un nombre completo, que se compone de un prefijo y un
nombre local separados por un signo de dos puntos. Tanto el prefijo como
los nombres locales deben ser un NCName. El prefijo debe estar asociado
con una referencia a un identificador URI de espacio de nombres,
mediante una declaración de espacio de nombres.</td>
</tr>
<tr class="even">
<td><code>NOTATION</code></td>
<td>Representa un conjunto de <code>QName</code>.</td>
</tr>
</tbody>
</table>

#### Derivados

<table style="width:99%;">
<colgroup>
<col style="width: 51%" />
<col style="width: 47%" />
</colgroup>
<thead>
<tr class="header">
<th>Tipo de dato</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>normalizedString</code></td>
<td>Representa cadenas normalizadas de espacios en blanco. Este tipo de
datos se deriva de <code>string</code>.</td>
</tr>
<tr class="even">
<td><code>token</code></td>
<td>Representa cadenas convertidas en símbolos. Este tipo de datos se
deriva de <code>normalizedString</code>.</td>
</tr>
<tr class="odd">
<td><code>language</code></td>
<td>Representa identificadores de lenguaje natural (definidos por RFC
1766). Este tipo de datos se deriva de <code>token</code>.</td>
</tr>
<tr class="even">
<td><code>IDREFS</code></td>
<td>Representa el tipo de atributo <code>IDREFS</code>. Contiene un
conjunto de valores de tipo <code>IDREF</code>.</td>
</tr>
<tr class="odd">
<td><code>ENTITIES</code></td>
<td>Representa el tipo de atributo <code>ENTITIES</code>. Contiene un
conjunto de valores de tipo <code>ENTITY</code>.</td>
</tr>
<tr class="even">
<td><code>NMTOKEN</code></td>
<td>Representa el tipo de atributo <code>NMTOKEN</code>.
<code>NMTOKEN</code> es un conjunto de caracteres de nombres (letras,
dígitos y otros caracteres) en cualquier combinación. A diferencia de
<code>Name</code> y <code>NCName</code>, <code>NMTOKEN</code>, no tiene
restricciones del carácter inicial. Este tipo de datos se deriva de
<code>token</code>.</td>
</tr>
<tr class="odd">
<td><code>NMTOKENS</code></td>
<td>Representa el tipo de atributo <code>NMTOKENS</code>. Contiene un
conjunto de valores de tipo <code>NMTOKEN</code>.</td>
</tr>
<tr class="even">
<td><code>Name</code></td>
<td>Representa nombres en XML. <code>Name</code> es un símbolo que
empieza con una letra, carácter de subrayado o signo de dos puntos, y
continúa con caracteres de nombre (letras, dígitos y otros caracteres).
Este tipo de datos se deriva de <code>token</code>.</td>
</tr>
<tr class="odd">
<td><code>NCName</code></td>
<td>Representa nombres sin el signo de dos puntos. Este tipo de datos es
igual que <code>Name</code>, excepto en que no puede empezar con el
signo de dos puntos. Este tipo de datos se deriva de
<code>Name</code>.</td>
</tr>
<tr class="even">
<td><code>ID</code></td>
<td>Representa el tipo de atributo <code>ID</code> definido en la
recomendación de XML 1.0. El <code>ID</code> no debe incluir un signo de
dos puntos (como <code>NCName</code>) y debe ser único en el documento
XML. Este tipo de datos se deriva de <code>NCName</code>.</td>
</tr>
<tr class="odd">
<td><code>IDREF</code></td>
<td>Representa una referencia a un elemento que tiene un atributo
<code>ID</code> que coincide con el <code>ID</code> especificado.
<code>IDREF</code> debe ser un <code>NCName</code> y tener un valor de
un elemento o atributo de tipo <code>ID</code> dentro del documento XML.
Este tipo de datos se deriva de <code>NCName</code>.</td>
</tr>
<tr class="even">
<td><code>ENTITY</code></td>
<td>Representa el tipo de atributo <code>ENTITY</code> definido en la
recomendación de XML 1.0. Es una referencia a una entidad sin analizar
con un nombre que coincide con el especificado. <code>ENTITY</code> debe
ser un <code>NCName</code> y declararse en el esquema como nombre de
entidad sin analizar. Este tipo de datos se deriva de
<code>NCName</code>.</td>
</tr>
<tr class="odd">
<td><code>integer</code></td>
<td>Representa una secuencia de dígitos decimales con un signo inicial
(<code>+</code> o <code>-</code>) opcional. Este tipo de datos deriva de
<code>decimal</code>.</td>
</tr>
<tr class="even">
<td><code>nonPositiveInteger</code></td>
<td>Representa un número entero menor o igual que cero.
<code>nonPositiveInteger</code> consta de un signo negativo
(<code>-</code>) y una secuencia de dígitos decimales. Este tipo de
datos se deriva de <code>integer</code>.</td>
</tr>
<tr class="odd">
<td><code>negativeInteger</code></td>
<td>Representa un número entero menor que cero. Consta de un signo
negativo (<code>-</code>) y una secuencia de dígitos decimales. Este
tipo de datos se deriva de <code>nonPositiveInteger</code>.</td>
</tr>
<tr class="even">
<td><code>long</code></td>
<td>Representa un número entero con un valor mínimo de
-9223372036854775808 y un valor máximo de 9223372036854775807. Este tipo
de datos se deriva de <code>integer</code>.</td>
</tr>
<tr class="odd">
<td><code>int</code></td>
<td>Representa un número entero con un valor mínimo de -2147483648 y un
valor máximo de 2147483647. Este tipo de datos se deriva de
<code>long</code>.</td>
</tr>
<tr class="even">
<td><code>short</code></td>
<td>Representa un número entero con un valor mínimo de -32768 y un valor
máximo de 32767. Este tipo de datos se deriva de <code>int</code>.</td>
</tr>
<tr class="odd">
<td><code>byte</code></td>
<td>Representa un número entero con un valor mínimo de -128 y un valor
máximo de 127. Este tipo de datos se deriva de <code>short</code>.</td>
</tr>
<tr class="even">
<td><code>nonNegativeInteger</code></td>
<td>Representa un número entero mayor o igual que cero. Este tipo de
datos se deriva de <code>integer</code>.</td>
</tr>
<tr class="odd">
<td><code>unsignedLong</code></td>
<td>Representa un número entero con un valor mínimo de cero y un valor
máximo de 18446744073709551615. Este tipo de datos se deriva de
<code>nonNegativeInteger</code>.</td>
</tr>
<tr class="even">
<td><code>unsignedInt</code></td>
<td>Representa un número entero con un valor mínimo de cero y un valor
máximo de 4294967295. Este tipo de datos se deriva de
<code>unsignedLong</code>.</td>
</tr>
<tr class="odd">
<td><code>unsignedShort</code></td>
<td>Representa un número entero con un valor mínimo de cero y un valor
máximo de 65535. Este tipo de datos se deriva de
<code>unsignedInt</code>.</td>
</tr>
<tr class="even">
<td><code>unsignedByte</code></td>
<td>Representa un número entero con un valor mínimo de cero y un valor
máximo de 255. Este tipo de datos se deriva de
<code>unsignedShort</code>.</td>
</tr>
<tr class="odd">
<td><code>positiveInteger</code></td>
<td>Representa un número entero mayor que cero. Este tipo de datos se
deriva de <code>nonNegativeInteger</code>.</td>
</tr>
</tbody>
</table>

## Tipo complejo: `complexType`

Sirve para definir elementos que tienen sub-elementos y/o atributos.

    <xs:complexType name="nombre_del_tipo_complejo">
      <xs:sequence> <!-- sequence/all/choice -->
        ... subelementos ...
      </xs:sequence>
      ... atributos ...
    </xs:complexType>

Puede contener elementos secundarios:

`sequence`  
Implica que deben aparecer todos los elementos y en ese orden (AND).

`all`  
Implica que deben aparecer todos los elementos, sin importar el orden.

`choice`  
Implica que sólo debe aparecer uno de esos elementos (OR).

`attribute`  
Para definir atributos.

Puede tener los siguientes atributos:

`name`  
Nombre del tipo complejo.

`mixed`  
Puede tener dos valores `true` o `false`.

`type`  
Tipo de datos con el que se identifica.

Dos posibles estructuras:

-   La primera contiene al tipo dentro de la estructura `element`:

        <xs:element name="contacto">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="destinatario" type="xs:string" />
              <xs:element name="remitente" type="xs:string" />
              <xs:element name="titulo" type="xs:string" />
              <xs:element name="contenido" type="xs:string" />
            </xs:sequence>
            <xs:attribute name="fecha" type="xs:date"/>
          </xs:complexType>
        </xs:element>

-   La segunda estructura lo que hace es primero definir el elemento con
    un tipo, y después definir fuera ese tipo:

        <xs:element name="contacto" type="tipo_contacto"/>

        <xs:complexType name="tipo_contacto">
          <xs:sequence>
            <xs:element name="destinatario" type="xs:string" />
            <xs:element name="remitente" type="xs:string" />
            <xs:element name="titulo" type="xs:string" />
            <xs:element name="contenido" type="xs:string" />
          </xs:sequence>
          <xs:attribute name="fecha" type="xs:date"/>
        </xs:complexType>

Note

La mejor opción es la segunda porque permite reutilizar ese tipo para
otros elementos. Además los parsers toleran mejor esta estructura.

### Elementos `sequence`, `all` y `choice`

Estos tres elementos nunca se utilizan juntos, aparece tan sólo uno de
ellos en el elemento `complexType`. Sirve para describir en qué orden y
cómo deben aparecer los subelementos del `complexType`.

Es equivalente a, en el DTD, poner comas o barras verticales en la
descripción de un elemento con hijos.

#### `sequence`

Este elemento indica que es obligatorio que aparezcan todos los
elementos especificados y en el orden en que se definen. Es el
equivalente a un AND.

En este ejemplo se define el elemento `libro`, con tres subelementos
obligatorios y que deben aparecer exactamente en este orden (1º
`titulo`, 2º `autor` y 3º `editorial`) y no aparecen es este orden o uno
de ellos no aparece, el parser produciría un error.

    <xs:element name="libro" type="tipo_libro"/>

    <xs:complexType name="tipo_libro">
      <xs:sequence>
        <xs:element name="titulo" type="xs:string" />
        <xs:element name="autor" type="xs:string" />
        <xs:element name="editorial" type="xs:string" />
      </xs:sequence>
    </xs:complexType>

    <libro>
      <titulo>El señor de los anillos</titulo>
      <autor>John Ronald Ruelen Tolkien</autor>
      <editorial>Tirant Lo Blanch</editorial>
    </libro>

#### `all`

Este elemento indica que es obligatorio que aparezcan todos los
elementos especificados y pero NO en el orden en que se definen.

En este ejemplo se define el elemento `libro`, con tres subelementos
obligatorios.

    <xs:element name="libro" type="tipo_libro"/>

    <xs:complexType name="tipo_libro">
      <xs:all>
        <xs:element name="titulo" type="xs:string" />
        <xs:element name="autor" type="xs:string" />
        <xs:element name="editorial" type="xs:string" />
      </xs:all>
    </xs:complexType>

    <libro>
      <titulo>El señor de los anillos</titulo>
      <editorial>Tirant Lo Blanch</editorial>
      <autor>John Ronald Ruelen Tolkien</autor>
    </libro>

#### `choice`

Este elemento indica que de todos los elementos especificados sólo debe
aparecer uno de ellos. Es el equivalente al OR.

En este ejemplo se define el elemento `libro`, con tres posibles
subelementos. Puede tener o un `titulo` o un `autor` o una `editorial`.

    <xs:element name="libro" type="tipo_libro"/>

    <xs:complexType name="tipo_libro">
      <xs:choice>
        <xs:element name="titulo" type="xs:string" />
        <xs:element name="autor" type="xs:string" />
        <xs:element name="editorial" type="xs:string" />
      </xs:choice>
    </xs:complexType>

    <libro>
      <titulo>El señor de los anillos</titulo>
    </libro>

### Elemento `attribute`

Para definir los atributos de un elemento o tipo de elemento utilizamos
la siguiente estructura:

    <xs:attribute name="nombre_atributo" type="tipo_atributo" use="modificador" />

Puede contener los siguientes atributos:

`name`  
Es el nombre del atributo.

`type`  
Es el tipo del atributo.

`use`  
Para definir si es un atributo obligatorio u opcional. Para definir un
atributo como obligatorio le asignaremos el valor `required`. Por
defecto es opcional.

La localización del atributo no puede ir por sí solo, ya que con esta
estructura no sabríamos a que elemento se refiere. Para ello se pone
siempre dentro de una estructura `complexType`.

Tipo simple: `simpleType` --------------------------

Un tipo simple sirve para definir una serie de restricciones a un
elemento o a un atributo. Es muy útil para definir rangos, tipos
enumerados, etc.

    <xs:simpleType name="nombre_del_tipo_simple">
      <xs:restriction>
        ... restricciones ...
      </xs:restriction>
    </xs:simpleType>

Puede contener elementos secundarios:

`restriction`  
Para poner rangos, patrones, enumerar posibles valores etc.

`list`  
Para definir un tipo de lista.

`union`  
Para unir varios tipos definidos anteriormente en uno.

Puede contener atributos:

`name`  
Para poner el nombre al tipo simple.

### Elemento `restriction`

Se utiliza para poner rangos, patrones enumerar posibles valores, etc.

    <xs:restriction base="xs:string">
      <xs:nombre_restriccion value=""/>
    </xs:restriction>

Tiene el atributo `base`. Es el tipo predefinido de datos sobre el que
se construye la restricción.

Puede contener las siguientes restricciones:

`enumeration`  
Se ponen los valores que puede tomar el elemento.

`maxExclusive`, `minExclusive`  
Valores mínimos o máximos que puede tomar el elemento, sin incluir el
último valor.

`maxInclusive`, `minInclusive`  
Valores mínimos o máximos que puede tomar el elemento, incluyendo el
último valor.

`pattern`  
Expresión regular que expresa la restricción.

    <xs:pattern value="([a-zA-Z0-9])*"/>

En este caso decimos que el patrón es de longitud indefinida, y que
puede contener letras mayúsculas, minúsculas y números.

    <xs:pattern value="\d{2}-\d{4}"/>

En este caso decimos que el patrón es de dos dígitos seguido de un guión
y otros 4 dígitos. Por ejemplo, 25-6789.

`length`, `maxLength`, `minLength`  
Longitud de un elemento de tipo texto.

`totalDigits`  
Número exacto de dígitos permitidos.

`fractionDigits`  
Número máximo de decimales permitidos.

También en este caso hay dos posibles estructuras:

-   La primera contiene al tipo dentro de la estructura element:

        <xs:element name="lista_de_enteros">
          <xs:simpleType>
            <xs:restriction base="xs:integer">
              <xs:minInclusive value="100"/>
              <xs:maxInclusive value="200"/>
            </xs:restriction>
          </xs:simpleType>
        </xs:element>

-   La segunda estructura lo que hace es 1º definir el elemento con un
    tipo, y después definir fuera ese tipo:

        <xs:element name="lista_de_enteros" type="tipo_lista_enteros"/>

        <xs:simpleType name="tipo_lista_enteros">
          <xs:restriction base="xs:integer">
            <xs:minInclusive value="100"/>
            <xs:maxInclusive value="200"/>
          </xs:restriction>
        </xs:simpleType>

Note

La mejor opción es la segunda porque permite reutilizar ese tipo para
otros elementos. Además los parsers toleran mejor esta estructura.

#### Ejemplos de restricciones

    <xs:element name="sexo" type="tipo_sexo"/>

    <xs:simpleType name="tipo_sexo">
      <xs:restriction base="xs:string">
        <xs:enumeration value="mujer"/>
        <xs:enumeration value="hombre"/>
      </xs:restriction>
    </xs:simpleType>  

    <xs:element name="codigo_postal" type="tipo_cp"/>

    <xs:simpleType name="tipo_cp">
      <xs:restriction base="xs:string">
        <xs:length value="5"/>
      </xs:restriction>
    </xs:simpleType>

    <xs:element name="password" type="tipo_password"/>

    <xs:simpleType name="tipo_password">
      <xs:restriction base="xs:string">
        <xs:pattern value="\d{3}-[A-Z]{2}"/>
      </xs:restriction>
    </xs:simpleType>

### Elemento `list`

Permite definir un tipo simple compuesto por una lista de otros tipos
simples separados siempre por espacios:

    <xs:simpleType name="lista_numeros">
      <xs:list itemType="xs:integer"/>
    </xs:simpleType>

### Elemento `union`

Permite combinar varios tipos simples en uno solo:

    <xs:simpleType name="entero_o_fecha">
      <xs:union memberTypes="xs:integer xs:date"/>
    </xs:simpleType>

## Extender un tipo

Utilizando `xs:extension` podemos ampliar un `simpleType` o
`complexType`, añadiendo elementos o atributos extra a un tipo base
definido anteriormente.

    <xs:complexType name="tipo_persona">
      <xs:sequence>
        <xs:element name="nombre" type="xs:string"/>
        <xs:element name="edad" type="xs:integer"/>
      </xs:sequence>
      <xs:attribute name="id" type="xs:integer"/>
    </xs:complexType>

    <xs:complexType name="tipo_contacto">
      <xs:complexContent>
        <xs:extension base="tipo_persona">
          <xs:sequence>
            <xs:element name="email" type="xs:string"/>
            <xs:element name="telefono" type="xs:string"/>
          </xs:sequence>
        </xs:extension>
      </xs:complexContent>
    </xs:complexType>

## Elementos sin subelementos

Mediante `simpleContent` podemos definir un elemento que solo pueda
contener texto y atributos, no subelementos.

    <xs:complexType name="tipo_documento">
      <xs:simpleContent>
        <xs:extension base="xs:string">
          <xs:attribute name="plantilla" type="xs:string" use="required"/>
          <xs:attribute name="revisado" type="xs:boolean" default="false"/>
        </xs:extension>
      </xs:simpleContent>
    </xs:complexType>

## Elementos vacíos

Para definir un elemento vacío, que no pueda tener ni texto ni
subelementos, basta con no poner ningún subelemento en la declaración
del tipo:

    <xs:complexType name="tipo_evento">
      <xs:attribute name="nombre" type="xs:string" use="required"/>
      <xs:attribute name="activo" type="xs:boolean" default="false"/>
    </xs:complexType>

## Convertir DTDs en esquemas XML

En un principio, con la creación de XML, se empezó empleando las DTDs
como modo de especificación de modelos; la existencia de más
herramientas para ello hizo que gran parte de las empresas que empezaron
a trabajar con XML adoptasen el uso de las DTDs. Actualmente el uso de
estas ha quedado más restringido en su uso, y se está empezando a
desarrollar de acuerdo al estándar de XML Schema; por ello, a
continuación, presentaremos las transformaciones que deberían realizarse
para convertir una DTD en un Schema.

En principio mostraremos a que elemento de XML Schema corresponden que
elementos de las DTDs, aunque existen herramientas de traducción
(DTD2HTML en Perl, XMLSpy, …) entre estos dos lenguajes, la siguiente
tabla intenta expresar como funciona con el fin de una mejor
comprensión.

<table style="width:99%;">
<colgroup>
<col style="width: 28%" />
<col style="width: 28%" />
<col style="width: 41%" />
</colgroup>
<thead>
<tr class="header">
<th>DTD</th>
<th>Schema</th>
<th>Comentarios</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><code>ELEMENT</code></td>
<td><code>&lt;element&gt;</code></td>
<td>Crea un vínculo entre un nombre y unos atributos, modelos de
contenido y anotaciones.</td>
</tr>
<tr class="even">
<td><code>#PCDATA</code></td>
<td></td>
<td>Soportado como parte de un tipo simple</td>
</tr>
<tr class="odd">
<td><code>ANY</code></td>
<td><code>&lt;any&gt;</code></td>
<td>Posee distintos comodines para un mayor conjunto de posibilidades.
Existe también <code>&lt;anyAttribute&gt;</code> con comodines
similares.</td>
</tr>
<tr class="even">
<td><code>EMPTY</code></td>
<td>Soportado</td>
<td>Se elimina la existencia de elementos descendientes del actual,
diferenciando de la presencia de un string vacío en un elemento.</td>
</tr>
<tr class="odd">
<td>Modelo de contenido</td>
<td><code>&lt;complexType&gt;</code></td>
<td></td>
</tr>
<tr class="even">
<td>Conector de secuencia (<code>,</code>)</td>
<td><code>&lt;sequence&gt;</code></td>
<td></td>
</tr>
<tr class="odd">
<td>Conector de alternativa (<code>|</code>)</td>
<td><code>&lt;choice&gt;</code></td>
<td></td>
</tr>
<tr class="even">
<td>Opcional (<code>?</code>)</td>
<td>Soportado</td>
<td>Se han de emplear los atributos predefinidos de
<code>maxOccurs</code> y <code>minOccurs</code></td>
</tr>
<tr class="odd">
<td>Requerido y Repetible (<code>+</code>)</td>
<td>Soportado</td>
<td>Se han de emplear los atributos predefinidos de
<code>maxOccurs</code> y <code>minOccurs</code></td>
</tr>
<tr class="even">
<td>Opcional y Repetible (<code>*</code>)</td>
<td>Soportado</td>
<td>Se han de emplear los atributos predefinidos de
<code>maxOccurs</code> y <code>minOccurs</code></td>
</tr>
<tr class="odd">
<td><code>ATTLIST</code></td>
<td><code>&lt;attributeGroup&gt;</code></td>
<td>Se pueden agrupar declaraciones de
<code>&lt;attributes&gt;</code></td>
</tr>
<tr class="even">
<td>Tipo de atributo <code>CDATA</code>, <code>ID</code>,
<code>IDREF</code>, <code>NOTATION</code> , …</td>
<td>Tipos <code>&lt;simpleType&gt;</code> predefinidos</td>
<td></td>
</tr>
<tr class="odd">
<td><code>ENTITY</code></td>
<td>No soportado</td>
<td>Las entidades son declaradas en declaraciones de marcas en el
XML.</td>
</tr>
<tr class="even">
<td><code>ENTITY%Parameter</code></td>
<td>No soportada</td>
<td></td>
</tr>
</tbody>
</table>

## Utilización del esquema

Para utilizar el esquema desde un documento XML, tenemos que tener en
cuenta si está en nuestro sistema de ficheros local o es un esquema
público.

-   En caso de que el esquema esté en un sitio público:

        <nodo_raiz xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.miempresa.com/mi_esquema.xsd">

-   En caso de que el esquema esté en local:

        <nodo_raiz xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:noNamespaceSchemaLocation="mi_esquema.xsd">
