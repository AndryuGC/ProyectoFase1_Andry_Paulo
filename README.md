MiniLang – Analizador Léxico
Curso: Compiladores  
Fase: Proyecto #1 – Analizador Léxico  
Lenguaje utilizado: Python  
Paulo Klee - 1078023


--Descripción del Proyecto

Este proyecto implementa el analizador léxico (scanner) para un subconjunto de un lenguaje estructurado llamado MiniLang.

El objetivo es reconocer los diferentes tokens del lenguaje, controlar la posición (línea y columna), manejar indentación significativa (INDENT / DEDENT) y reportar errores léxicos sin detener la ejecución del programa.

El analizador recibe como entrada un archivo con extensión `.mlng` y genera como salida un archivo `.out` con el listado de tokens encontrados.

--Características Implementadas

--Palabras clave
- int
- float
- bool
- string
- if
- else
- while
- Read
- Write
- return

--Identificadores (ID)
- Inician con letra o "_"
- Pueden contener letras, números y "_"
- Longitud máxima: 31 caracteres
- Si excede el límite:
- Se reporta error
- Se trunca a los primeros 31 caracteres

Expresión regular conceptual:
[a-zA-Z_][a-zA-Z0-9_]{0,30}


--Literales numéricos
- Enteros → 123
- Flotantes → 3.14

Expresiones regulares conceptuales:
INT   → [0-9]+  
FLOAT → [0-9]+\.[0-9]+  


--Cadenas de texto
- Delimitadas por comillas dobles " "
- No pueden contener salto de línea
- Si no se cierran → error léxico

Expresión regular conceptual:
"([^"\n])*"


--Operadores

Aritméticos:
+  -  *  /

Relacionales:
>  <  ==  !=  >=  <=

Asignación:
=


--Símbolos especiales

( ) : ,


--Manejo de Indentación

MiniLang utiliza bloques definidos por indentación (similar a Python).

Se implementó una pila de niveles de indentación:

- El nivel inicial es 0.
- Si la indentación aumenta → se emite INDENT y se apila el nuevo nivel.
- Si disminuye → se emiten uno o más DEDENT.
- Si el nivel no coincide con ningún nivel previo → se reporta error de indentación.

Se permiten hasta 5 niveles de indentación.

--Token NEWLINE

Cada línea válida genera un token NEWLINE.

Este token es significativo porque el lenguaje usa indentación para definir bloques.

--Manejo de Errores Léxicos

El programa no se detiene al encontrar errores.

Errores detectados:

- Carácter inesperado
- Cadena sin cerrar
- Identificador demasiado largo
- Indentación inválida

Formato de error:

line <n>, col <m>: ERROR descripción

Los errores se muestran en pantalla, mientras que los tokens se escriben en el archivo `.out`.



PROYECTOFASE01/
│
├── PROYECTOFASE01.py
├── test1.mlng
├── test2.mlng
├── test3.mlng
├── test4.mlng
├── test5.mlng
└── README.md

Ejecución

Desde la terminal:

python minilang.py

Luego ingresar el nombre del archivo .mlng cuando el programa lo solicite.

Ejemplo:

Ingrese archivo .mlng: test1.mlng

El programa generará automáticamente:

test1.out

--Salida

El archivo .out contiene los tokens en orden de aparición, con:

- Tipo de token
- Valor (si aplica)
- Línea
- Columna inicial y final

Ejemplo:

WRITE(Write) [1:1-5]  
STRING(Hola Mundo) [1:7-19]  
NEWLINE [1:20-20]  
EOF [1:1-1]

--Gramática Libre de Contexto (BNF)

<program> ::= <statement_list>

<statement_list> ::= <statement> NEWLINE <statement_list>
                   | <statement>

<statement> ::= <declaration>
              | <assignment>
              | <if_statement>
              | <while_statement>

<declaration> ::= <type> ID  
<type> ::= int | float | bool | string  

<assignment> ::= ID "=" <expression>  

<if_statement> ::= "if" <expression> ":" INDENT <statement_list> DEDENT  
                 | "if" <expression> ":" INDENT <statement_list> DEDENT  
                   "else" ":" INDENT <statement_list> DEDENT  

<while_statement> ::= "while" <expression> ":" INDENT <statement_list> DEDENT  

<expression> ::= <expression> "+" <term>  
               | <expression> "-" <term>  
               | <term>  

<term> ::= <term> "*" <factor>  
         | <term> "/" <factor>  
         | <factor>  

<factor> ::= ID  
           | INT  
           | FLOAT  
           | STRING  
           | "(" <expression> ")"

---

--Conclusión

El analizador léxico cumple con todos los requerimientos de la Fase 1:

- Reconoce correctamente los tokens
- Maneja indentación significativa
- Controla línea y columna
- Reporta errores sin detener la ejecución
- Genera archivo de salida adecuado
