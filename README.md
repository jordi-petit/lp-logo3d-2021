# Logo3D

Aquesta pàgina descriu la segona pràctica de GEI-LP (edició 2020-2021 Q2).
La vostra tasca és implementar un intèrpret d'un llenguatge de programació
anomenat Logo3D per permetre pintar amb una tortuga en 3D.

DIBUIX


## Història (adaptada de la Wikipedia)

LOGO és un llenguatge de programació desenvolupat per cap el 1967 a Cambridge,
Massachusetts amb l'objectiu d'explorar el camp de l'"ensenyament assistit per
ordinador". LOGO és un llenguatge que permet ensenyar a programar als infants
a través del joc i l'experimentació, introduint conceptes de matemàtiques,
geometria cartesiana i trigonometria de forma intuïtiva. Mitjançant els
gràfics de tortuga programats, els infants aprenien a programar i fins i tot
podien arribar de forma molt intuïtiva a utilitzar conceptes de programació
avançada, com la recursivitat, el tractament (rudimentari) de llenguatge
natural i els processos d'inferència lògica.


## El llenguatge Logo3D: Tutorial

El llenguatge de programació Logo3D modernitza el LOGO clàssic adoptant una
nova i elegant sintàxi i situant la popular tortuga en un entorn 3D!

Per exemple, aquesta forma geomètrica en escala

DIBUIX

es pot obtnir amb aquest programa:

```
PROC escala() IS
    FOR i FROM 1 TO 20 DO
        forward(10)
        up(90)
        forward(2)
        down(90)
        right(90)
    END
END
```

El següent programa mostra com llegir dos nombres i calcular el seu màxim comú
divisor utilitzant l'algorisme d'Euclides en Logo3D utilitzant dos
procediments i entrada/sortida:

```
// Programa principal.
PROC main() IS
    >> valor1 >> valor2
    euclides(valor1, valor2)
END

// Escriu el mcd de a i de b.
PROC euclides(a, b) IS
    WHILE a != b DO
        IF a > b THEN
            a := a - b
        ELSE
            b := b - a
        END
    END
    << a
END
```

Les variables són locals a cada invocació de cada procediment i els
procediments es poden comunicar a través de pas de paràmetres per valor.

Les variables no han de ser declararades, i són totes de tipus real.
Fixeu-vos que Logo3D utilitza l'autèntic operador d'assignació
que mai s'hauria d'haver abandonat:
el `:=`.

Com es veu a l'exemple, la sintàxi per llegir i escriure és utilitzant `>>` i
`<<` respectivament. Com que les instruccions no es separen ni acaben amb punts
i comes estupids, `>> a >> b` sembla un encadenament d'operacions però, en
realitat, són dues instruccions executades l'una rera l'altra.

Els comentaris comencen amb `//` i acaben al final de la seva línia.

A banda d'invocar procediments escrits per l'usuari, també es poden invocar
procediments que controlen la tortuga. Per exemple, aquest procediment

```
PROC quadrat_blau(mida) IS
    color(0.2, 0.2, 1)
    FOR i FROM 1 TO 4 DO
        forward(mida)
        left(90)
    END
END
```

dibuixa un quadrat amb un delicat to blau (donat amb `color()`)
de la mida requerida tot movent la tortuga endavant (`forward`) i a l'esquerra
(`left`) quatre cops:

DIBUIX

La tortuga comença a l'orígen de les coordenades mirant horitzontalment cap a
la dreta i amb color roig. El seu rumb es pot canviar amb `left()` i `right()`
en un angle  i amb `up()` i `down()` per l'altre angle. Les unitats d'angle es
dónen en graus. En l'estat normal, quan la tortuga avança o retrocedeix
(`forward` o `backward`), aquesta deixa un rastre del darrer color triat amb
`color` (que funciona amb valors RGB entre 0 i 1).  Cridant a `hide()`, la
tortuga deixa de pintar al moure's, amb `show()` torna a pintar. Es pot
recol·locar la tortuga al punt d'orígen amb `home()`.


## La vostra feina

La vostra feina consisteix en implementar un intèrpret de Logo3D que permeti
pintar amb una tortuga en 3D. Per escriure l'intèrpret heu d'utilitzar Python
i l'ANTLR, tal com s'ha explicat a les classes de laboratori. Per crear les
escenes 3D utilitzareu `vpython`.  Per realitzar la vostra pràctica,  només
podeu utilitzar llibreries estàndards de Python, ANTLR i vpython.

Us proposem planificar el vostre treball en tres fases:

1. Realització de l'intèrpret de Logo3D (sense tortuga).

2. Implementar una classe Turtle3D que doni suport a la creació
   d'escenes 3D a través d'una tortuga (totalment independent de l'intèrpret).

3. Extendre l'intèrpret per tal que permeti usar la classe Turtle3D.

Fixeu-vos que les fases 1 i 2 són totalment independents i no és fins a la
fase 3 que es posen en comú. Si heu separat bé 1 de 2, aquesta integració
hauria de ser ben senzilla.

A continuació s'especifiquen amb més detall els elements necessaris.


## Especificació de Logo3D

Les instruccions de Logo3D són:

- l'assignació,
- la lectura,
- l'escriptura,
- el condicional,
- la iteració amb `WHILE`,
- la iteració amb `FOR`, i
- la invocació a un procediment.


### Assignació

L'assignació ha d'avaluar primer l'expressió a la part dreta del `:=` i
enmagatzemar després el resultat a la variable local a la part esquerra.


### Lectura

La instrucció de lectura ha de llegir un valor real del canal d'entrada
estàndard  i enmagatzemar-lo a la variable a la dreta del `>>`.


### Escriptura

La instrucció d'escriptura ha d'avaluar l'expressió a la dreta del `<<` i
escriure-la, en una línia, al canal de sortida estàndard.


### Condicional

La instrucció condicional té la semàntica habitual. El bloc `ELSE` és optatiu.


### Iteració amb `WHILE`

La instrucció iterativa amb `WHILE` té la semàntica habitual.


### Iteració amb `FOR`

La instrucció iterativa amb `FOR` té la semàntica habitual, tenint en compte
que els valors d'inici i de final es calculen abans d'iterar. Compte: El valor
de la variable de control pot ser canviat dins del cos de la iteració.


### Invocació de procediment


La crida a procediments té la semàntica habitual.  Els paràmetres es passen
per valor, avaluant les expressions abans. Si el nombre de paràmetres passats
no corresponen als declarats, es produeix un error. Els procediments no són
funcions i no poden retornar resultats. Però els procediments es poden cridar
recursivament.


### Expressions

Si una variable encara no ha rebut cap valor, el seu valor és zero. Els
operadors aritmètics són els habituals (`+`, `-`, `*`, `/`) i amb la mateixa
prioritat que en matemàtiques. Evidentment, es poden usar parèntesis. El
operadors relacionals (`==`, `!=`, `<`, `>`, `<=`, `>=`) retornen zero per
fals i u per cert. Quan cal interpretar un valor com a booleà (als `WHILE`s i
`IF`s), zero és fals, qualsevol altre valor és cert.


### Àmbit de visibilitat

No importa l'ordre de declaració dels procediments. Les variables són locals a
cada invocació de cada procediment. No hi ha variables globals ni manera
d'accedir a variables d'altres procediments.


### Errors

Malgrat que Logo3D és força senzill, els programadors poden realitzar molts
errors. Per aquesta pràctica, només us demanem que detecteu els errors més
verosímils (divisió per zero, crida a procediment no definit, repetició de
procediment ja definit, nombre de paràmetres incorrectes, noms de paràmetres
formals repetits, ...) i aborteu el programa amb una excepció quan es dónen.


### Invocació

El vostre intèrpret s'ha d'invocar amb la comanda `python3 logo3d.py` tot
passant-li com a paràmetre el nom del fitxer que conté el codi font. Per exemple:

```bash
python3 logo3d.py programa.l3d
```

Els programes poden començar des de qualsevol procediment.  Per defecte, es
comença pel procediment `main`.
Si es vol començar el programa des d'un procediment diferent de `main()`, cal donar el
seu nom com a segon paràmetre i es poden passar els valors dels seus paràmetres (nombre reals)
des de la linia de comandes.

```bash
python3 logo3d.py programa.l3d quadrats 10 20
```

### Extensions

Podeu extendre el llenguatge amb construccions del vostre gust, a condició de mantenir
una compatibilitat estricta amb l'especificació donada. A més, cal que documenteu
amb precisió les vostres extensions.

Per exemple, podríeu extendre Logo3D amb operadors lògics,
funcions que retornin valors, amb variables de
tipus text, ...

Compte: Les extensions poden portar molta feina, consulteu-ho abans amb el vostre professor.


## Especificació de la classe Turtle3D

TBD


# Lliurament

TBD





