FROM php:8.4-cli

# Instala dependencias del sistema y extensiones necesarias
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       git \
       unzip \
       libzip-dev \
       zlib1g-dev \
       libonig-dev \
       libxml2-dev \
       ca-certificates \
    && docker-php-ext-install pdo_mysql mbstring zip pcntl bcmath \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Copia Composer desde la imagen oficial para tenerlo disponible
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

# Copia el código y ejecuta la instalación de dependencias PHP
COPY . /var/www/html

RUN if [ -f composer.json ]; then composer install --prefer-dist --no-progress --no-suggest --no-interaction || true; fi

EXPOSE 8000

# Comando por defecto: sirve la app Laravel en 0.0.0.0:8000
CMD ["php", "artisan", "serve", "--host=0.0.0.0", "--port=8000"]
