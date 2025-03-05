# Windons systeme 
You can use the code to build chatbot streaming output, I only used the agent chunks and I didn't use supervisor chunks \
, the only thing you need to modify is in Backend/langgraph_agnet/views.py\
use your owe key.\

os.environ['OPENAI_API_KEY'] = ''\
os.environ['TAVILY_API_KEY'] = ''
# Result showing


https://github.com/user-attachments/assets/252f86e9-d485-451f-abed-287ca6362721


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
pip install channels\

In settings.py, add langchain_stream and daphne to INSTALLED_APPS:\

Warning: `daphne` must be listed before django.contrib.staticfiles in INSTALLED_APPS.\

'daphne',\
'channels',\
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
I set the key in views.py , you should change your key
os.environ['OPENAI_API_KEY'] = ''
os.environ['TAVILY_API_KEY'] = ''


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
## vue setting
please see the code, change app.vue code, in  component folder use ChatComponent.vue.\
I change the index.html this code "'\<link rel="icon" type="image/svg+xml" href="/vite.svg" />'" to use my favorite icon, \
the changed code :"\<link rel="icon" type="image/svg+xml" href="/src/assets/socks.ico" />"\
I add this icon named socks.ico in src/assests folder

# RUN SERVER

In different terminal:\
npm run dev\
python .\manage.py runserver





