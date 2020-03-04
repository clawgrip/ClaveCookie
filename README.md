# ClaveCookie
Gestor de cookies para compatibilidad de Cl@ve con navegadores y "SameSite".

A partir del 17 de febrero de 2020, el navegador Chrome empieza a desplegar progresivamente su nueva versión 80, que maneja las cookies de manera diferente. De forma resumida, para incrementar la seguridad, Chrome bloqueará las cookies que, en una redirección, se reciban sin el atributo "SameSite" correctamente configurado. Para entender en detalle este comportamiento puede consultarse la siguiente URL: 
https://blog.chromium.org/2019/10/developers-get-ready-for-new.html
El resto de navegadores seguirá los pasos de Chrome en versiones posteriores.
Estas modificaciones tienen las siguientes consecuencias detectadas:
* Para los SP que utilicen los kit de integración estándar de Clave: no se han observado consecuencias, funcionando la autenticación satisfactoriamente.
* Además, una vez en Clave si el usuario utiliza un método de identificación distinto de certificado digital (ClavePIN, ClavePermanente o EIDAS), cuando se despliegue la versión de Clave modificada no existirán consecuencias en este sentido por los cambios introducidos, hasta que esté desplegada la nueva versión (prevista para la semana del 24 de febrero), tampoco existirán consecuencias siempre que el proceso de login se realice antes de los 2 minutos desde que se iniciara la redirección a los sistemas de la AEAT o la SS.
En todos los casos, se recomienda encarecidamente que se realice una prueba en cada SP activando los dos flags implicados, tal como se explica a continuación, para forzar en el navegador el comportamiento de la versión 80 de Chrome en referencia a las cookies, y de ese modo confirmar fehacientemente que el comportamiento de cada SP a la hora de autenticar a los usuarios es el esperado.

Para simular el comportamiento de los navegadores con la nueva funcionalidad:

A- Chrome
   - Versiones anteriores a v80 es posible forzar este comportamiento activando el flag "same-site-by-default-cookies" y “cookies-without-same-site-must-be-secure” en chrome://flags, de forma que se puede comprobar el funcionamiento que tendrán las aplicaciones por defecto una vez que el cambio se haga efectivo.
   - Versiones posteriores a v80 y v80 tendrán el flag "same-site-by-default-cookies" en chrome://flags activado.
 
B- Firefox:
    - Versiones anteriores a v69 activando el flag "network.cookie.sameSite.laxByDefault" en about:config, de forma que se puede comprobar el funcionamiento que tendrán las aplicaciones por defecto una vez que el cambio se haga efectivo.
    - Versiones posteriores a v69 y v69 tendrán el flag "network.cookie.sameSite.laxByDefault" en about:config activado.
  
C- Edge:
   - Versiones posteriores a v80 y v80 (Chromium) es posible forzar este comportamiento activando el flag "same-site-by-default-cookies" en edge://flags, de forma que se puede comprobar el funcionamiento que tendrán las aplicaciones por defecto una vez que el cambio se haga efectivo.

A la hora de realizar las pruebas se puede obtener información utilizando las Herramientas para desarrolladores del navegador. Se recomienda tener activo el check "Preserve log" de la consola donde aparecerían los errores por cookies bloqueadas (por ejemplo "A cookie associated with a cross-site resource at ...") y comprobar los valores de los distintos atributos de las cookies que se pueden ver en "Application > Storage > Cookies".
 
En el caso de que haya alguna cookie de la aplicación que se esté bloqueando y que sea necesaria para su correcto funcionamiento, se recomienda fijar el atributo SameSite a None por requerir que la cookie sea enviada en invocaciones a sitios cruzados, es necesario añadir también el atributo Secure, ya que de lo contrario también sería rechazada por no viajar en una conexión segura: "Set-Cookie: third_party_var=value; SameSite=None; Secure".

