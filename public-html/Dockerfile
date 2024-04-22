# Utiliza la imagen de ASP.NET con Apache
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base

# Instala herramientas y dependencias necesarias
RUN apt-get update && apt-get install -y nano wget gnupg2

# Configura las variables de entorno para configuración adicional
#ENV SOURCE=pre-net 
ENV ENDPOINT=mysql-net-pre
ENV DATABASE=proyecto
ENV USERD=user
ENV PASSD=u00947hyASPdfkl_111NEThs-pa44s55w--?7
#ENV SIMPLE=simple-net

# Copia los archivos del proyecto al contenedor
RUN mkdir -p /var/www/net/public_content
RUN mkdir -p /var/www/private_content
COPY . /var/www/net
#RUN mv /var/www/net/simplesaml /var/www/simplesaml

# Habilita mod_rewrite en Apache
COPY 000-default.conf  /etc/apache2/sites-available/000-default.conf

# Configura los permisos de las carpetas para www-data
RUN chown -R www-data:www-data /var/www/net/public_content && \
    chown -R www-data:www-data /var/www/private_content && \
    chmod -R 770 /var/www/net/public_content && \
    chmod -R 770 /var/www/private_content
#RUN service apache2 restart

# Configura la zona horaria
RUN ln -snf /usr/share/zoneinfo/America/Chihuahua /etc/localtime && echo America/Chihuahua > /etc/timezone

# Limpia la caché de paquetes y los archivos temporales
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Expone los puertos utilizados por la aplicación
EXPOSE 80
EXPOSE 9003


# Establece el directorio de trabajo
WORKDIR /var/www/html/
