# Utiliza la imagen de ASP.NET con Apache
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base

# Instala herramientas y dependencias necesarias
RUN apt-get update && apt-get install -y nano wget gnupg2

# Configura las variables de entorno para configuración adicional
ENV SOURCE=pre-net 
ENV ENDPOINT=mysql-net-pre
ENV DATABASE=proyecto
ENV USERD=user
ENV PASSD=u00947hyASPdfkl_111NEThs-pa44s55w--?7
ENV SIMPLE=simple-net

# Copia los archivos del proyecto al contenedor
RUN mkdir -p /var/www/net/public_content
RUN mkdir -p /var/www/private_content
COPY . /var/www/net
RUN mv /var/www/net/simplesaml /var/www/simplesaml

# Habilita las sesiones
RUN echo "session.save_path=\"/tmp\"" >> /usr/local/etc/php/php.ini

# Instala el controlador xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
COPY ./config/xdebug/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/

# Instala el controlador de PHP para SQL Server
RUN cat /etc/os-release \
  && apt-get update \
  && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
  && curl https://packages.microsoft.com/config/ubuntu/22.04/prod.list \
      > /etc/apt/sources.list.d/mssql-release.list \
  && apt-get install -y --no-install-recommends apt-transport-https \
  && apt-get update \
  && ACCEPT_EULA=Y apt-get -y --no-install-recommends install unixodbc-dev msodbcsql17

# Instala las extensiones de PHP necesarias
RUN docker-php-ext-install calendar
RUN docker-php-ext-install pdo_mysql \
  && pecl install sqlsrv pdo_sqlsrv \
  && docker-php-ext-enable sqlsrv pdo_sqlsrv

# Habilita mod_rewrite en Apache
COPY 000-default.conf  /etc/apache2/sites-available/000-default.conf
RUN a2enmod rewrite && \
    echo "display_errors = Off" >> /usr/local/etc/php/php.ini && \
    echo "log_errors = On" >> /usr/local/etc/php/php.ini
RUN service apache2 restart

# Configura la zona horaria
RUN ln -snf /usr/share/zoneinfo/America/Chihuahua /etc/localtime && echo America/Chihuahua > /etc/timezone

# Limpia la caché de paquetes y los archivos temporales
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Expone los puertos utilizados por la aplicación
EXPOSE 80
EXPOSE 9003

# Establece el directorio de trabajo
WORKDIR /var/www/html/
