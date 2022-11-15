# Sentinela

# Instalando
sudo apt install python3-pip python3 python3-dev python3-venv gcc default-libmysqlclient-dev libssl-dev nginx curl

# Atualizando pip e demais
python3.7 -m pip install --upgrade pip setuptools wheel --user
export PATH="/home/$USER/.local/bin:$PATH"
python3.7 -m pip install pipenv --user

# Criar pasta
mkdir agenda
# Entrar na pasta
cd agenda

# Criando o ambiente virtual
python3.7 -m venv venv
source venv/bin/activate
python3.7 -m pip install django gunicorn pillow
