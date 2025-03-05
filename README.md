# Set virtual environment
python -m pipenv shell
# Backend
## Install django
pip install django
### Create django project
django-admin startproject Backend
### Move into the project directory
cd Backend
### Create a Django App named
python manage.py startapp langchain_stream
