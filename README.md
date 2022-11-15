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
