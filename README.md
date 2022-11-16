# Sentinela

# Instalando
sudo apt install python3-pip python3 python3-dev python3-venv gcc default-libmysqlclient-dev libssl-dev nginx curl

# Atualizando pip e demais
python3 -m pip install --upgrade pip setuptools wheel --user

export PATH="/home/$USER/.local/bin:$PATH"

python3 -m pip install pipenv --user

# Criar pasta
mkdir sentinela

# Entrar na pasta
cd sentinela

# Criando o ambiente virtual
python3 -m venv venv

source venv/bin/activate

python3 -m pip install django gunicorn pillow

# Clonando repositório
git clone xxxxxxxxxxx

# Passos seguintes

pip install -r dependencias.txt

python manage.py migrate

python manage.py createsuperuser

python manage.py runserver

# Rodar o Servidor

gunicorn --bind 0.0.0.0:8005 guardiao.wsgi



-----------------------------------------------------------------------------


# Criando o arquivo
sudo nano /etc/systemd/system/gunicorn.socket
##################################################

# COLAR (SEM EDIÇÃO)
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
##################################################

# Criando outro arquivo
sudo nano /etc/systemd/system/gunicorn.service
##################################################

# Editar, depois Colar
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=###############USUARIO###############
Group=www-data
WorkingDirectory=/home/###############USUARIO###############/agenda
ExecStart=/home/###############USUARIO###############/agenda/venv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          agenda.wsgi:application

[Install]
WantedBy=multi-user.target
##################################################

# Ativando
sudo systemctl start gunicorn.socket
sudo systemctl enable gunicorn.socket
##################################################

# Checando
sudo systemctl status gunicorn.socket
sudo systemctl status gunicorn
curl --unix-socket /run/gunicorn.sock localhost
sudo systemctl status gunicorn
##################################################

sudo nano /etc/nginx/sites-enabled/sitedjango
##################################################

# Configurando o nginx server block
server {
    listen 80;
    server_name localhost;

    location = /favicon.ico { access_log off; log_not_found off; }
    
    location /static/ {
        root /home/luizotavio/agenda;
    }

    location /media {
        alias /home/luizotavio/agenda/media/;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}

##################################################

sudo rm /etc/nginx/sites-enabled/default
sudo systemctl restart nginx
sudo systemctl restart gunicorn
##################################################

