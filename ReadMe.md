목차
-----
[01. 프로젝트 초기환경 설정](#01--프로젝트-초기환경-설정)

[02. Bootstrap4를 활용한 레이아웃 구현](#02.-Bootstrap4를-활용한-레이아웃-구현)

## 01.  프로젝트 초기환경 설정

* python env 를 이용해 가상환경 셋팅

  ```python
  python -m venv venv
  pip install django~=3.0.0
  ```

* 프로젝트 생성 (폴더 만들고 들어간다음에 생성할 것)

  ```
  django-admin startproject askcompanay .
  ```

* DB migration 하고 superuser 계정을 만든다. 

  ```
  python manage.py migrate
  python manage.py createsuperuser
  ```

* settings.py 에 STATIC  및 MEDIA 기본 경로 지정

  ```python
  TEMPLATES = [
      {
          'DIRS': [
              os.path.join(BASE_DIR, 'askcompany', 'templates'),
          ],
      },
  ]
  ###############
  STATIC_URL = '/static/'
  STATICFILES_DIRS =[
      os.path.join(BASE_DIR,'askcompany','templates'),
  ]
  STATIC_ROOT = os.path.join(BASE_DIR, 'static')
  
  MEDIA_URL ='/media/'
  MEDIA_ROOT = os.path.join(BASE_DIR, 'meida')
  
  INTERNAL_IPS =['127.0.0.1']
  ```

* static 폴더와 templates 폴더를 askcompany 폴더 밑에 생성한다. 

  * templates 폴더 밑에 layout.html을 생성한다.

  * static 폴더 밑에  jquery-3.6.1.min.js  파일을 다운로드 받아서 넣어 놓는다.

* urls.py에 media File에 대한 static serve 를 위해 url 셋팅

  ```python
  if settings.DEBUG:
      urlpatterns += static(settings.MEDIA_URL,
                            document_root=settings.MEDIA_ROOT)
  ```

* debug-toolbar를 설치한다. (공식문서 참조하여 설치하면 됨)

  ```python
  pip install debug-toolbar
  
  # urls.py
  if settings.DEBUG:
      import debug_toolbar
      urlpatterns +=[
          path('__debug__/', include(debug_toolbar.urls))
      ]
      
      
  #settings.py
  INSTALLED_APPS = [
      'debug_toolbar',
  ]
  
  MIDDLEWARE = [
      'debug_toolbar.middleware.DebugToolbarMiddleware',
  ]
  
  ```

* settings 폴더 셋팅

  * settings 폴더안에 settings.py파일을 common.py로 저장한다.
  * dev.py, prod.py 파일도 만든다.
  * common.py 에서 경로를 다음과 같이 수정

  ```python
  #BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  #dirname은 부모경로, abspath는 절대경로
  BASE_DIR = dirname(dirname(dirname(abspath(__file__))))
  ```

* django settings 경로 설정

  * settings  폴더에서 `__init__.py` 파일 생성
  * manage.py에서 다음과 같이 셋팅

  ```python
  #manage.py
  os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'askcompany.settings.dev')
  ```

  * asgi.py 및 wsgi.py에서 다음과 같이 셋팅 

  ```python
  os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'askcompany.setting.prod')
  ```

  

[목차](#목차)

## 02. Bootstrap4를 활용한 레이아웃 구현

* bootstrap4.x.x 다운로드 및 static으로 복사,  jquery-3.x.x.min.js 다운로드후 복사

* urls.py에 템플릿 뷰 삽입

  ``` python
  urlpatterns = [
      path('',TemplateView.as_view(template_name='root.html'), name='root'),
  ]
  ```

* root.html  구현

  ```html
  {% extends "layout.html" %}
  
  {% block content %}
      <div class="container">
          <div class="row">
              <div class="col-sm-12">
                  Instagram
              </div>
          </div>
      </div>
  {% endblock %}
  ```

* root.html의 부모 html 인 layout.html 구현 

  * https://getbootstrap.com/docs/4.4/examples/pricing/ 을 참조해서 구현한다. 




[목차](#목차)















> 본 메뉴얼은 인프런의 이진석 강사님의  [파이썬/장고 웹서비스 완벽 개발 가이드 with 리액트](https://www.inflearn.com/course/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9E%A5%EA%B3%A0-%EC%9B%B9%EC%84%9C%EB%B9%84%EC%8A%A4)를 정리한 문서 입니다.
