### django REST framework

> RESTful API 서버를 쉽게 구축할 수 있도록 도와주는 오픈소스 라이버르리



- Serializer
  - client에게 전달하기 위한 Format
  - 기존) django ORM QuerySet -> Template -> HTML로 렌더링
  -  REST) QuerySet -> JSON



---



#### serializer 시작하기



- App 안에 serializers.py 생성

  - serializers.py

    ```python
    from django.contrib.auth.models import User, Group
    from rest_framework import serializers
    
    
    class UserSerializer(serializers.HyperlinkedModelSerializer):
        class Meta:
            model = User
            fields = ['url', 'username', 'email']
    ```

    

  - rest_views.py

    ```python
    from django.contrib.auth.models import User, Group
    from rest_framework import viewsets
    from rest_framework import permissions
    from blog.serializers import UserSerializer
    
    class UserViewSet(viewsets.ModelViewSet):
        queryset = User.objects.all().order_by('-date_joined')
        serializer_class = UserSerializer
        permission_classes = [permissions.IsAuthenticated]
    ```

    

- url 등록

  - urls.py

    ```python
    from rest_framework import routers
    from blog import rest_views
    
    router = routers.DefaultRouter()
    router.register('users', rest_views.UserViewSet)
    urlpatterns = [
        path('api/', include(router.urls)),
        path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
    ]
    
    ```



- setting 등록

  - settings.py

    ```python
    INSTALLED_APPS = [
    'rest_framework',  # app 등록
    ]
    REST_FRAMEWORK = {
        'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
        'PAGE_SIZE': 10 
    }
    ```

    