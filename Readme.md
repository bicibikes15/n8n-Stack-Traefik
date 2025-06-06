# Stack de n8n para Traefik by DigitAllFran

Este stack despliega n8n, el maestro de la automatización de flujos de trabajo, en una configuración de alto rendimiento (`queue-mode`) diseñada para ser gestionada y asegurada por un ecosistema de servicios preexistente.

## Arquitectura y Dependencias

Este no es un stack solitario. Es un general de élite que requiere de un ejército y un arsenal para funcionar. Su poder reside en su integración con otras fortalezas que debes haber desplegado previamente.

#### **Prerrequisitos Indispensables:**

1.  **El Guardián (Traefik):** Necesitas una instancia de Traefik funcionando como proxy inverso.
    * **Stack Recomendado:** [Stack de Traefik de DigitAllFran](https://github.com/bicibikes15/Traefik)

2.  **El Arsenal (Bases de Datos):** n8n requiere PostgreSQL y Redis para operar. Este stack se conectará al:
    * **Stack Global de Bases de Datos de DigitAllFran](https://github.com/bicibikes15/Globals-Databases)

3.  **La Red Compartida:** Todos los stacks deben estar conectados a la misma red externa. El estandarte oficial de este ecosistema es `digitallfran_net`.

#### **Gestión Opcional:**

-   **La Atalaya (Portainer):** Para gestionar este y otros stacks desde una interfaz gráfica, puedes usar:
    * **[Stack de Portainer de DigitAllFran](https://github.com/bicibikes15/Portainer-Stack-Traefik)**

## El Ritual de Invocación (Instalación)

Sigue estos pasos en orden para desplegar tu fortaleza de automatización.

**1. Clonar el Repositorio**
```bash
# Reemplaza 'tu-github' con tu nombre de usuario y 'n8n-stack' con el nombre de tu repo
git clone [https://github.com/tu-github/n8n-stack.git](https://github.com/tu-github/n8n-stack.git)
cd n8n-stack

2. Verificar la Red Externa

Asegúrate de que la red digitallfran_net existe. Si no, créala:

Bash

docker network create digitallfran_net

3. Preparar la Base de Datos (Paso Crucial)

Para garantizar un arranque sin errores, crearemos manualmente la base de datos para n8n dentro de tu contenedor de PostgreSQL global.

Ejecuta este comando en la terminal de tu servidor:

Bash

docker exec -it postgres_global psql -U postgres -c "CREATE DATABASE n8n_database;"
La terminal debería devolver CREATE DATABASE, confirmando que el terreno está listo.

4. Configurar los Secretos y Dominios

Crea tu archivo de configuración local a partir de la plantilla.

Bash

cp .env.example .env
Ahora, edita el archivo .env (nano .env) y configura todas las variables. Las más importantes son:

N8N_DOMAIN: El dominio para el editor, ej. n8n.tudominio.com. (Debe apuntar a la IP de tu servidor).
N8N_WEBHOOK_DOMAIN: El dominio para los webhooks, ej. n8nwh.tudominio.com. (Debe apuntar a la misma IP).
DB_PASSWORD: La contraseña de tu PostgreSQL global.
REDIS_PASSWORD: La contraseña de tu Redis global.
N8N_ENCRYPTION_KEY: Una nueva clave segura para n8n (puedes generarla en Linux con openssl rand -hex 32).

5. Desplegar n8n

Con los prerrequisitos listos y el .env configurado, invoca el jutsu final.

Bash

docker compose up -d
Verificación
Traefik detectará el nuevo contenedor, le asignará certificados SSL y lo hará disponible en los dominios que configuraste. Podrás acceder al editor de n8n en https:// seguido de tu N8N_DOMAIN.