Compiladores
2025-

## Evaluación continua 1 - Prueba 5

# 1. Objetivo

El objetivo es diseñar e implementar un visitor para un lenguaje imperativo con sentencias
de control y expresiones booleanas.

# 2. Indicaciones

Se cuenta con un scanner, parser y visitor que trabajan sobre la siguiente gramática:

```
Program ::= StmtList
StmtList ::= Stmt (′;′ Stmt)∗
Stmt ::= id′=′ CExp
| print′(′CExp′)′
| if CExp then StmtList [ else StmtList ] endif
| while CExp do StmtList endwhile
```
```
CExp ::= Bexpr [< Bexpr]
BExp ::= expr {(+|−) expr}∗
expr ::= term{(∗| /) term}∗
term ::= f actor [∗∗ f actor]
f actor ::= number | (CExp) | id
```
Esta gramática define un mini-lenguaje imperativo que permite construir programas com-
puestos por una lista de sentencias separadas por punto y coma. Ejemplos de programas
válidos:

```
Ejemplo 1 – Asignación y operación aritmética
```
```
x = 5 + 3 * 2 ;
print(x)
```
```
Ejemplo 2 – Condicional simple
```
```
x = 10 ;
if x < 20 then
print(x)
else
print(0)
endif
```


```
Ejemplo 3 – Bucle while
```
```
n = 5 ;
while n < 10 do
n = n + 1 ;
print(n)
endwhile
```
```
Ejemplo 4 – Expresiones con potencia y paréntesis
```
```
y = (2 + 3) ** 2 ;
print(y)
```
```
Ejemplo 5 – Condicional anidado con operaciones
```
```
a = 4 * 2 ;
b = a - 3 ;
if b < 5 then
print(b)
else
while b < 15 do
b = b + 2 ;
print(b)
endwhile
endif
```
La gramática debe ser extendida de modo que el scanner, el AST, el parser y el visitor
soporten las siguientes construcciones adicionales:

```
Program ::= StmtList
StmtList ::= Stmt (; Stmt)∗
Stmt ::= id = AExp
| print ( AExp )
| if AExp then StmtList [ else StmtList ] endif
| while AExp do StmtList endwhile
| do StmtList while AExp enddo
```

```
AExp ::= BExp [(and| or) BExp]
```
```
BExp ::= CExp [(<| >) CExp]
```
```
CExp ::= DExpr {(+|−) DExpr}∗
```
```
DExpr ::= T erm{(∗| /) T erm}∗
```
```
T erm ::= F actor [∗∗ F actor]
```
```
F actor ::= number | true | false | not (AExp) | (AExp) | id
```
Ejemplos de cadenas válidas con las nuevas reglas:

```
Ejemplo 1: do–while con not
```
```
x = 0;
do
print(x);
x = x + 1
while not (x > 2)
enddo
```
```
Ejemplo 2: do–while con and y or
```
```
x = 1;
y = 5;
do
print(x * y);
x = x + 1
while (x < 4 and not (y < 3)) or false
enddo
```
```
Ejemplo 3: If–Else con operación lógica
```
```
x = 4;
y = 10;
if (x < y and y > 5) then
print(x * y)
else
print(y + x)
endif
```

Compiladores
2025-

```
Ejemplo 4: While con operaciones booleanas
```
```
x = 0;
while (x < 5 or false) do
print(x);
x = x + 1
endwhile
```
```
Ejemplo 5: Do–While anidado con If–Else
```
```
x = 1;
y = 2;
do
if (x < y) then
print(x * y);
x = x + 1
else
print(y - x);
y = y + 1
endif
while (x < 5 and y < 10)
enddo
```
# 3. Sugerencias:


Los valores true y false deben ser admitidos dentro de NumberExp, asignándoles
los valores enteros 1 y 0 respectivamente.


El procedimiento recomendado es: primero agregar los tokens, luego ajustar el
escanner; posteriormente modificar el AST, después actualizar el parser y, final-
mente, el visitor.

Para extender el AST es necesario crear dos nuevas clases:

- DoWhile como subclase de Stm, que represente la construcción do–while.
- NotExp como subclase de Exp, que modele la operación unaria not.


