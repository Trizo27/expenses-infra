# expenses-infra

Repositorio de infraestructura para **Spendly** — gestor de gastos personales.

## Arquitectura

- **Frontend**: servido por Nginx desde `/var/www/lab06.42m.work/front/dist/`
- **Backend**: proceso Node gestionado por Supervisor en puerto 3000
- **Base de datos**: PostgreSQL corriendo en el servidor
- **SSL**: certificados Let's Encrypt generados con Certbot

## Requisitos

- Ansible instalado en tu máquina
- Acceso SSH al servidor
- Python 3 en el servidor

## Instalación de roles

```bash
ansible-galaxy install -r requirements.yml -p roles/
```

## Playbooks

| Playbook | Descripción |
|---|---|
| `configure-server.yml` | Crea usuario deploy y carpetas en `/var/www/` |
| `configure-node.yml` | Instala nvm + Node v20 para el usuario deploy |
| `configure-postgres.yml` | Instala y configura PostgreSQL + base de datos |
| `configure-nginx.yml` | Instala Nginx + configura el sitio |
| `configure-certbot.yml` | Genera certificado SSL con Let's Encrypt |
| `configure-supervisor.yml` | Instala Supervisor + configura el backend como servicio |

## Orden de ejecución

```bash
# 1. Crear usuario y carpetas
ansible-playbook -i hosts.yml playbooks/configure-server.yml

# 2. Instalar Node
ansible-playbook -i hosts.yml playbooks/configure-node.yml

# 3. Instalar PostgreSQL
ansible-playbook -i hosts.yml playbooks/configure-postgres.yml

# 4. Instalar y configurar Nginx
ansible-playbook -i hosts.yml playbooks/configure-nginx.yml

# 5. Generar certificado SSL
ansible-playbook -i hosts.yml playbooks/configure-certbot.yml

# 6. Configurar Supervisor para el backend
ansible-playbook -i hosts.yml playbooks/configure-supervisor.yml
```

## Variables

Todas las variables del proyecto están en `vars/dev.yml`:

```yaml
deploy_user: deploy           # usuario del servidor
domain: lab06.42m.work        # dominio del sitio
use_nodejs_version: 20.19.3   # versión de Node
postgres_db_name: expenses_db # nombre de la base de datos
```

## Entorno local

Para levantar el proyecto localmente con Docker:

```bash
# Backend + DB
cd ../expenses-backend
docker compose up -d

# Frontend
cd ../expenses-frontend
docker compose up -d
```

Todo disponible en `http://localhost`
