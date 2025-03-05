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
python manage.py startapp langgraph_agent

### Configure the Django settings.py for Websockets
pip install daphne \

In settings.py, add langchain_stream and daphne to INSTALLED_APPS:\

Warning: `daphne` must be listed before django.contrib.staticfiles in INSTALLED_APPS.\

'daphne',\
 ...,\
'langgraph_agent',
### Replace the WSGI application line with an ASGI configuration to enable asynchronous communication.
Remove or comment out the line:\
#WSGI_APPLICATION = ' Backend.wsgi.application'\
Add the following ASGI configuration line:\
ASGI_APPLICATION = "Backend.asgi.application"

### Create the views.py file
Please see the code
pip install -U langchain_community langchain_anthropic langchain_experimental\
pip install langchain-openai\
pip install langgraph==0.2.73\

### Set Up Websocket Routing
Define how websocket connections are handled by creating routing.py and urls.py in your langgraph_agent app.\

Create the file: langgraph_agent/routing.py,and add the following code:\
from django.urls import re_path  \
from . import views  \
  
websocket_urlpatterns = [  
    re_path(r'ws/chat/$', views.ChatConsumer.as_asgi()),  
]\

Create the file: langgraph_agent/urls.py,and add the following code:\
from django.urls import path  \
from . import views  \
  
  
urlpatterns = [  
    path('ws/chat/', views.ChatConsumer.as_asgi()),  
]\

Replace the code in Backend/asgi.py with the following:\
import os  \
from django.core.asgi import get_asgi_application  \
from channels.routing import ProtocolTypeRouter, URLRouter  \
from channels.auth import AuthMiddlewareStack  \
import langgraph_agent.routing  \
  
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'Backend.settings')  \
  
application = ProtocolTypeRouter({  \
  "http": get_asgi_application(),  \
  "websocket": AuthMiddlewareStack(  \
        URLRouter(  \
            langgraph_agent.routing.websocket_urlpatterns  \
        )  \
    ), \ 
})\

# Frontend
Create a Frontend folder in the Backend same directory \
cd Frontend
## install vue
npm create vite@latest\
Name the project frontend, select 'Vue' as the framework, and choose 'JavaScript' for the variant. Then, navigate into your new frontend directory:\
cd frontend\
Install the required React packages:\
npm install


