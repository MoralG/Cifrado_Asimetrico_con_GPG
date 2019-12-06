### Tarea 1: Generación de claves 
-------------------------------------------------------------
#### 1. Genera un par de claves (pública y privada). ¿En que directorio se guarda las claves de un usuario?

###### Las claves de un usuario se guardan en una base de datos en el directorio */home/moralg/.gnupg/openpgp-revocs.d*

###### Vamos a generar el certificado con el que firmaremos las claves
~~~
gpg --gen-key
    gpg (GnuPG) 2.2.12; Copyright (C) 2018 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Nota: Usa "gpg --full-generate-key" para el diálogo completo de generación de clave.

    GnuPG debe construir un ID de usuario para identificar su clave.

    Nombre y apellidos: Alejandro Morales Gracia
    Dirección de correo electrónico: ale95mogra@gmail.com
    Ha seleccionado este ID de usuario:
        "Alejandro Morales Gracia <ale95mogra@gmail.com>"

    ¿Cambia (N)ombre, (D)irección o (V)ale/(S)alir? v
    Es necesario generar muchos bytes aleatorios. Es una buena idea realizar
    alguna otra tarea (trabajar en otra ventana/consola, mover el ratón, usar
    la red y los discos) durante la generación de números primos. Esto da al
    generador de números aleatorios mayor oportunidad de recoger suficiente
    entropía.
    Es necesario generar muchos bytes aleatorios. Es una buena idea realizar
    alguna otra tarea (trabajar en otra ventana/consola, mover el ratón, usar
    la red y los discos) durante la generación de números primos. Esto da al
    generador de números aleatorios mayor oportunidad de recoger suficiente
    entropía.
    gpg: clave 318AD3BFD821894E marcada como de confianza absoluta
    gpg: certificado de revocación guardado como '/home/moralg/.gnupg/openpgp-revocs.d/ 70EB4283C6407E0E14C6CA8F318AD3BFD821894E.rev'
    claves pública y secreta creadas y firmadas.

    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          70EB4283C6407E0E14C6CA8F318AD3BFD821894E
    uid                      Alejandro Morales Gracia <ale95mogra@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~

###### Ahora vamos a exportar las clave publica y privada

~~~
gpg --export -a Alejandro Morales Gracia > pub-miclave.key
gpg --export-secret-keys -a Alejandro Morales Gracia > priv-miclave.key
~~~


#### 2. Lista las claves públicas que tienes en tu almacén de claves. Explica los distintos datos que nos muestra. ¿Cómo deberías haber generado las claves para indicar, por ejemplo, que tenga un 1 mes de validez?

###### Para indicar la fecha de validez es con la opcion *gpg --full-generate-key* para que te pregunte a la hora de crearla la fecha de validez

~~~
gpg --list-key
    /home/moralg/.gnupg/pubring.kbx
    -------------------------------
    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          70EB4283C6407E0E14C6CA8F318AD3BFD821894E
    uid        [  absoluta ] Alejandro Morales Gracia <ale95mogra@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]

~~~

##### Salida:

###### - En la primera linea de la publica se indica cuando fue creada la clave publica y la fecha cuando caduca
###### - Luego viene el certificado que ha firmado dichas claves
###### - El uid te indica la información del usuario que ha certificado dichas claves
###### - Y la última lineas indicaba las subkeys

#### 3. Lista las claves privadas de tu almacén de claves

~~~
gpg --list-secret-keys
    /home/moralg/.gnupg/pubring.kbx
    -------------------------------
    sec   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          70EB4283C6407E0E14C6CA8F318AD3BFD821894E
    uid        [  absoluta ] Alejandro Morales Gracia <ale95mogra@gmail.com>
    ssb   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~

### Tarea 2: Importar / exportar clave pública
-------------------------------------------------------------
#### 1. Exporta tu clave pública en formato ASCII y guardalo en un archivo nombre_apellido.asc y envíalo al compañero con el que vas a hacer esta práctica

~~~
gpg --export -a Alejandro Morales Gracia > alejandro_morales.asc
~~~

#### 2. Importa las claves públicas recibidas de vuestro compañero

~~~
gpg --import Luis_Vazquez.asc
~~~

#### 3. Comprueba que las claves se han incluido correctamente en vuestro keyring

~~~
gpg --list-public-keys
    /home/moralg/.gnupg/pubring.kbx
    -------------------------------
    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          70EB4283C6407E0E14C6CA8F318AD3BFD821894E
    uid        [  absoluta ] Alejandro Morales Gracia <ale95mogra@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]

    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          ED536410153671FE10EF9C6A30B2CE7D46D49C6B
    uid        [desconocida] Luis Vazquez Alejo <luisvazquezalejo@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
~~~

### Tarea 3: Cifrado asimétrico con claves públicas
-------------------------------------------------------------
#### 1. Cifraremos un archivo cualquiera y lo remitiremos por email a uno de nuestros compañeros que nos proporcionó su clave pública

~~~
touch prueba.txt

gpg -e -u "Alejandro Morales Gracia" -r "Luis Vazquez Alejo" prueba.txt
    gpg: EDDAA98D51C90308: No hay seguridad de que esta clave pertenezca realmente
    al usuario que se nombra

    sub  rsa3072/EDDAA98D51C90308 2019-10-07 Luis Vazquez Alejo     <luisvazquezalejo@gmail.com>
     Huella clave primaria: ED53 6410 1536 71FE 10EF  9C6A 30B2 CE7D 46D4 9C6B
          Huella de subclave: 92FC 1B65 46B3 423A 1C45  8F09 EDDA A98D 51C9 0308

    No es seguro que la clave pertenezca a la persona que se nombra en el
    identificador de usuario. Si *realmente* sabe lo que está haciendo,
    puede contestar sí a la siguiente pregunta.

    ¿Usar esta clave de todas formas? (s/N) s
~~~

#### 2. Nuestro compañero, a su vez, nos remitirá un archivo cifrado para que nosotros lo descifremos

#### 3. Tanto nosotros como nuestro compañero comprobaremos que hemos podido descifrar los mensajes recibidos respectivamente

~~~
gpg -d luisgpg.txt.gpg
    gpg: cifrado con clave de 3072 bits RSA, ID 80DBA261303BFBC3, creada el 2019-10-07
          "Alejandro Morales Gracia <ale95mogra@gmail.com>"
    salu2
~~~

#### 4. Por último, enviaremos el documento cifrado a alguien que no estaba en la lista de destinatarios y comprobaremos que este usuario no podrá descifrar este archivo

###### El error que le sale a un remitente que no esta en la lista sería el siguiente

~~~
$ gpg -d /tmp/mozilla_paloma0/prueba.txt.gpg
gpg: cifrado con clave RSA, ID EDDAA98D51C90308
gpg: descifrado fallido: No secret key
~~~ 

#### 5. Para terminar, indica los comandos necesarios para borrar las claves públicas y privadas que posees

##### Clave pública

~~~
gpg --delete-key "Nombre de Usuario"
~~~

##### Clave privada
~~~
gpg --delete-secret-key "Nombre de Usuario"
~~~

### Tarea 4: Exportar clave a un servidor público de claves PGP
-------------------------------------------------------------
#### 1. Genera la clave de revocación de tu clave pública para utilizarla en caso de que haya problemas

###### Para realizar la clave de revocación necesitamos la ID de la clave pública y ahora hacemos:
~~~
gpg --list-public-keys
    /home/moralg/.gnupg/pubring.kbx
    -------------------------------
    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          70EB4283C6407E0E14C6CA8F318AD3BFD821894E
    uid        [  absoluta ] Alejandro Morales Gracia <ale95mogra@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
    
    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          ED536410153671FE10EF9C6A30B2CE7D46D49C6B
    uid        [desconocida] Luis Vazquez Alejo <luisvazquezalejo@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]
    
    pub   rsa3072 2019-10-07 [SC] [caduca: 2019-11-06]
          FD52AA5481FD0E1518011D83952393CC31351D81
    uid        [desconocida] Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2019-11-06]

gpg --output revoke-miclave.asc --gen-revoke 70EB4283C6407E0E14C6CA8F318AD3BFD821894E

    sec  rsa3072/318AD3BFD821894E 2019-10-07 Alejandro Morales Gracia   <ale95mogra@gmail.com>

    ¿Crear un certificado de revocación para esta clave? (s/N) s
    Por favor elija una razón para la revocación:
      0 = No se dio ninguna razón
      1 = La clave ha sido comprometida
      2 = La clave ha sido reemplazada
      3 = La clave ya no está en uso
      Q = Cancelar
    (Probablemente quería seleccionar 1 aquí)
    ¿Su decisión? 1
    Introduzca una descripción opcional; acábela con una línea vacía:
    > Para realizar una tarea de Seguridad
    >  
    Razón para la revocación: La clave ha sido comprometida
    Para realizar una tarea de Seguridad
    ¿Es correcto? (s/N) s
    se fuerza salida con armadura ASCII.
    Certificado de revocación creado.

    Por favor consérvelo en un medio que pueda esconder; si alguien consigue
    acceso a este certificado puede usarlo para inutilizar su clave.
    Es inteligente imprimir este certificado y guardarlo en otro lugar, por
    si acaso su medio resulta imposible de leer. Pero precaución: ¡el sistema
    de impresión de su máquina podría almacenar los datos y hacerlos accesibles
    a otras personas!

ls
    miclave-priv.key  miclave-pub.asc  miclave-pub.key  revoke-miclave.asc
~~~

#### 2. Exporta tu clave pública al servidor pgp.rediris.es

###### Ahora exportamos al servidor pgp.rediris.es para tenerla almacenada para que la descargue quien quiera.

~~~
gpg --keyserver pgp.rediris.es --send-keys EC7EEAB6F3986FD97F72A4409F229BC929C410C4
    gpg: enviando clave 9F229BC929C410C4 a hkp://pgp.rediris.es
~~~

#### 3. Borra la clave pública de alguno de tus compañeros de clase e impórtala ahora del servidor público de rediris

###### Borramos la clave pública y la importamos de nuevo:
~~~
gpg --delete-key Paloma R. Garcia Campon
    gpg (GnuPG) 2.2.12; Copyright (C) 2018 Free Software Foundation, Inc.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.


    pub  rsa3072/952393CC31351D81 2019-10-07 Paloma R. Garcia Campon    <palomagarciacampon08@gmail.com>

    ¿Eliminar esta clave del anillo? (s/N) s
    gpg: clave "R." no encontrada: Not found
    gpg: R.: delete key failed: Not found

gpg --list-public-keys
    /home/moralg/.gnupg/pubring.kbx
    -------------------------------
    pub   rsa3072 2019-10-07 [SC] [caduca: 2021-10-06]
          ED536410153671FE10EF9C6A30B2CE7D46D49C6B
    uid        [desconocida] Luis Vazquez Alejo <luisvazquezalejo@gmail.com>
    sub   rsa3072 2019-10-07 [E] [caduca: 2021-10-06]

    pub   rsa3072 2019-10-14 [SC] [caduca: 2021-10-13]
          EC7EEAB6F3986FD97F72A4409F229BC929C410C4
    uid        [  absoluta ] Alejandro Morales Gracia <ale95mogra@gmail.com>
    sub   rsa3072 2019-10-14 [E] [caduca: 2021-10-13]

gpg --keyserver pgp.rediris.es ---search-keys Paloma R. Garcia Campon
    gpg: opción inválida "---search-keys"
    moralg@padano:~/Cifrado$ gpg --keyserver pgp.rediris.es --search-keys Paloma R. Garcia  Campon
    gpg: data source: http://130.206.1.8:11371
    (1)	Paloma R. Garcia Campon <palomagarciacampon08@gmail.com>
    	  3072 bit RSA key 952393CC31351D81, creado: 2019-10-07, caduca: 2019-11-06
    Keys 1-1 of 1 for "Paloma R. Garcia Campon".  Introduzca número(s), O)tro, o F)in > 1
    gpg: clave 952393CC31351D81: clave pública "Paloma R. Garcia Campon     <palomagarciacampon08@gmail.com>" importada
    gpg: Cantidad total procesada: 1
    gpg:               importadas: 1
~~~