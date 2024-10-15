# "Django Rest Framework" ga kirish

Virutal muhit yaratish
```bash
python3 -m venv .venv
python -m venv .venv
```

Virutal muhitni aktivlashtirish
```bash
sourve .venv/bin/activate
.venv\Scripts\Activate
```

Kutubxonalarni o'rnatish
```bash
pip install django djangorestframework
```


"PROJECT" yaratish loyiha yaratish
```python
django-admin startproject PROJECT .
```

"api" loyiha yaratish
```python
django-admin startapp app_api
```

"settings.py" da rest_frameowrk ni ro'yxatga olish
```python
INSTALLED_APPS = [
    ...
    'app_api',
    'rest_framework',
]
```

"PROJECT.urls" fayli ichida qo'shimcha yo'nalish yaratish va view ga bog'lash
```python
from django.urls import include

from . import views


urlpatterns = [
    ...
    path('api/', include('app_api.urls'))
]
```

"app_api.urls" ichida kerakli yo'nalishlarni yaratish
```python
from django.urls import path

from . import views


urlpatterns = [
    path('', views.index_page)
]
```

"app_api.views" ichida index_page funksiyasini yaratish va birinchi javobni berish
```python
from rest_framework.response import Response
from rest_framework.decorators import api_view

@api_view(['GET'])
def index_page(request):
    return Response(data={'hello': 'hi'})
```


Migratsiyalarni amalaga oshirish
```python
python manage.py migrate
```

Serverni ishga tushirish
```python
python manage.py runserver
```

Kerakli url bo'yicha o'tish
```text
http://localhost:8000/api/
```

PostgreSQL omborini ishlatish uchun kutubxoa o'rnatish
```python
pip install psycopg2-binary
```

Ma'lumotlar omborini PostgreSQL ga o'zgaritirish, "settings.py" faylida
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'n49',  # Ombor nomi, sizda boshqacha bo'lishi mumkin
        'USER': 'postgres',
        'PASSWORD': '123', # Ombor paroli, sizda boshqacha bo'lishi mumkin, PgAdmin o'rnatishda beriladigan parol
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```


Migratsiylarni boshidan bajarish
```python
python manage.py migrate
```

User larni JSON ga o'girib beradigan serializer yaratish, "app_app/serializers.py" ichida
```python
from rest_framework.serializers import ModelSerializer
from django.contrib.auth import get_user_model


User = get_user_model()


class UserSerializer(ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'username', 'email', 'first_name', 'last_name']
```

User larni olib beradigan ViewSet yozish, "app_api/views.py" ichida
```python
from rest_framework.viewsets import ModelViewSet
from django.contrib.auth import get_user_model

from . import serializers

User = get_user_model()

class UsersViewSet(ModelViewSet):
    queryset = User.objects.all()
    serializer_class = serializers.UserSerializer
```


Router yaratish
```python
from rest_framework.routers import DefaultRouter

from . import views

router = DefaultRouter()
router.register(prefix='users', viewset=views.UsersViewSet, basename='users')

urlpatterns = router.urls
```
