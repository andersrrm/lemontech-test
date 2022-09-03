# php-fix-csv
Upload CSV
1. Pasos que siguió para encontrar el error: 
    a. Como trabajo con multiples frameworks y sistemas operativos, tuve que crear un virtual host para este proyecto.
        - Fork & clone del proyecto hacia /var/www
        - cd /etc/apache2/sites-available 
        - sudo nano lemontech-test.conf
        -   <VirtualHost *:80>
                ServerName 127.0.0.3
                ServerAlias testphp.com
                ServerAdmin anders.rojas@pucp.pe
                DocumentRoot /var/www/lemontech-test/public
                <Directory /var/www/lemontech-test/public>
                        Options -Indexes +FollowSymLinks
                        AllowOverride All
                </Directory>

                ErrorLog ${APACHE_LOG_DIR}/domain1.com-error.log
                CustomLog ${APACHE_LOG_DIR}/domain1.com-access.log combined
            </VirtualHost>
        - sudo a2ensite leomntech-test.conf
        - sudo systemctl reload apache2
        - sudo apachectl configtest
        - sudo systemctl restart apache2
    b. Instalé las dependencias:
        - composer install
    c. Verifiqué última version del archivo:
        - git pull origin
    d. Revise el index.php para conocer la estructura del proyecto.
    e. Abrí el host 127.0.0.3 y verifiqué que había un error luego del cargar el archivo csv.
    f. Revise la página de la vista y me di cuenta que el FOREACH LOOP no podía ser usado de esa forma ya que el archivo CSV leído contiene $keys & $values. Posteriormente, modifiqué los FORECH LOOPS, agregando indexes en el header and body.
    g. Faltaba el reserved word para  <? php after de los FOREACH LOOP por lo que también me apresuré a colocarlos.
    h. Cambié la sentencia </th> por </tr> luego del FOREACH LOOP ya que estaba equivocada.
    i. Para verificar los totales de las columnas y no invertir mucho tiempo sumando el resultado de cada fila primero imprimi una alerta desde php para corroborar si la suma de rates despues del FOREACH LOOP era la misma que estaba siendo calculada: 
        echo '<script>alert('.$this->rates.')</script>'; 
    j. Luego identifiqué el error al hacer un cálculo básico de 100 columnas * un promedio de rate= 50 (hacían vacías y valor a 90), el resultado debería ser igual a 50000+- 20%. Lo cual difería enormente del dato mostrado. Entonces revisé la columna leída correjí los indexes de $taxes y $rates para que puedan ser leídas correctamente.
    k. Finalmente, modifiqué el código de cómo se calcula el $total, para lidear lo menos posible con indexes numéricos y sea más entendible el código.
2. Redactar respuesta a equipo de soporte sobre el ticket mencionado.
    Dear team, 
    Just fixed the bug and tested the complete cycle in the Lemontech-test project. 
    Please, give it a check when available and let me know to redeploy it ASAP. 
    BTW, let the client know, this update will make the web faster and won't compromise stored information from clients.
    Anders.
3. Posibles re-factor al proceso actual teniendo como objetivo evitar el mismo error a futuro.(Realizar un fork al repositorio y proporcionar la url debajo)
    url: https://github.com/andersrrm/lemontech-test
4. Revisión del proyecto en general enumerando inconsistencias visualzidas.
    - El projecto es funcional si se utiliza solo para el objetivo creado, hace un buen uso de los objetos pero es lento e inseguro. En caso de ampliar el proyecto y agregar páginas, información y datos programados, se sugiere utilizar un framework.
    - El proyecto no contiene comentarios.
    - El proyecto no filtra ni discrimina la extensión del archivo a ser cargado.
    - El proyecto no limita el peso del archivo a ser cargado.
    - El proyecto acepta solo valores positivos en RATE y TAX y no filtra valores flotantes o negativos.
    - El proyecto no limita la lectura de archivos maliciosos.
    - El proyecto no es responsive.
    - El proyecto no muestra mensajes de error o de confirmación.
    - El proyecto invía archivos por medio del POST METHOD sin autentificación ni token.
    - El proyecto renderiza desde el servidor y no tiene cache.
    - El proyecto no tiene branches de desarrollo adiciconales.
    - El proyecto carga dependencias innecesarias.
    - EL proyecto no contiene .gitignore.
