1. criar ambiente virtual

```ps1
python -m venv venv
.\venv\Scripts\activate
```

2. instalar os requisitos

```ps1
pip install djangorestframework
pip install markdown
pip install django-filter
```

3. criar projeto

```ps1
django-admin startproject nome_do_projeto .
```

`cd nome_do_projeto` ( se somente se nao tiver colocado . no final do codigo cli anterior)

```ps1
python manage.py startapp nome_do_app
```

ir em api, e registrar o app (nome da pasta) 
no `settings.py` em `INSTALLED_APPS`


4. criar db, superusuario e rodar servidor

```ps1
python manage.py migrate
python manage.py createsuperuser
python manage.py runserver
```

5. registrar `rest_framework` em `settings.py INSTALLED_APPS`

6. registrar as urls em `api`

```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('api-auth/', include('rest_framework.urls')),
]
```

- as coisas funcionam assim no roteamento url: 
    - rota -> view -> model -> serializer

7. criacao da model

```python
class Cliente (models.Model):
nome = models.CharField(max_length=50)
endereco = models.CharField(max_length=50)
idade = models.IntegerField()

def __str__(self):
return self.nome
```

8. registrar model no admin

```python
from .models import Cliente

admin.site.register(cliente)
```

9. dentro do `core` criar `serializers.py`

```py
from rest_framework import serializers
from .models import Cliente

class ClienteSerializer(serializers.ModelSerializer):
    class Meta:
        model = ClienteSerializer
        fields = ['id', 'nome', 'endereco', 'idade']
```

10. em `core` `views.py` definir as viewsets
```py
from rest_framework import viewsets
from .models import Cliente
from .serializers import ClienteSerializer

class ClienteViewSet(viewsets.ModelViewSet):
    queryset = Cliente.objects.all() #queryset quando der get retorna tudo oq queryset pegar
    serializer_class = ClienteSerializer
```

11. definir rotas em `api` `urls.py`

```py
from rest_framework import routers
from core.views impor ClienteViewSet

router = routers.DefaultRouter()
router.register(r'clientes', ClientesViewSet)
```

12. registrar as rotas nas `url patterns` em `api` `urls.py`

```py
path('', include(router.urls)),
```
