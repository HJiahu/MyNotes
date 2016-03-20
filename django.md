### ��װdjango
-	��װpip 
-	��python��Ŀ¼�µ�script�ļ���д�뻷������
-	���նˣ�`pip install django`

### һЩ����
-	ʹ��`python manage.py check`������磺
-	�������`Segmentation fault`����ִ��`python manage.py check`������ȹش���

### django��������
-	 һ����ͼ����Python��һ��������һ�㽫��ͼ�ļ������ƶ�Ϊ��veiws.py
-	 ������settings.py�иı�ʱ����TIME_ZONE����

### һ������
#### ����������Ӧ��
-	������Ӧ�õ�����
	-	Ӧ�ã�apps�������ĳЩ����
	-	���������ú�apps�ļ��ϡ�
-	���̵Ĵ�����`django-admin startproject mysite `#�ڵ�ǰĿ¼�´���һ����Ϊmysite ��վ�㣬ע�����Ʋ��ܺ�ϵͳ�е���������������ȡ��test��

```
mysite/        #��Ŀ¼
    manage.py  
    mysite/         
        __init__.py    #����ļ�һ��Ϊ�գ���Ϊ����python����ǰ�ļ��е���һ��������
        settings.py
        urls.py
        wsgi.py
```
-	����һ��Ӧ�ã�`python manage.py startapp polls`#Ӧ����Ϊpolls
-	Ϊ���ڹ����м���Ӧ�ã���Ҫ�ڹ����ļ��е�settings.py�е�INSTALLED_APPS�����Ӧ�õ����ƣ�` d`

```
polls/    #��manage.py��ͬĿ¼��polls���Լ��������ļ�
    __init__.py
    admin.py
    apps.py
    migrations/  #���ڴ���й����ݿ�����ݣ��������ݿ⽻����sql���
        __init__.py
    models.py
    tests.py
	urls.py   #���ĵ������Զ������ģ���Ҫ�ֶ���ӡ�ÿһ��app���������Լ���URLconf����ȫ��urls.py��ʹ��include����ʹ�ô�urls.py
    views.py
```
-	�ڹ��������app���༭mysite/urls.py 
-	**ע������include��ʹ�á�**ʹ��include�����Կ����������ʽ������djangoӦ�ã���Щ����Ƕ����ġ�
-	urlpatterns�е�url()�п������ĸ����������������Ǳ���ģ������ǿ�ѡ�ġ�

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^polls/', include('polls.urls')),#�����include��ʾ��ʹ��polls��urls.py ϵͳ����ȡurl��...polls/����ַ����ݸ�poll.urls
    url(r'^admin/', admin.site.urls),#urls�еĵ�һ������ʱurlƥ��������ʽ���ڶ��������ǽ�Ҫ���õ���ͼ����Ӧ�ĺ����� 
]
```
#### ����server
-	`python manage.py runserver`#Ĭ�϶˿ں�Ϊ��8080��ʹ��localhost:8080��127.0.0.1:8080���ʵ�ǰ��վ
-	` python manage.py runserver 8080`#�޸�Ĭ�϶˿�
-	`python manage.py runserver 0.0.0.0:8000`

### ���ݴ���
-	djangoĬ��ʹ��SQLite��Ϊ���ݿ⣬����ʹ���������ݿ���Ҫ��װ���������ݿ�ӿڡ�
-	�ڴ���django����ʱϵͳ��settings.py�е�INSTALLED_APPS�������һЩ�����Ӧ�ã���ЩӦ������Ҫ���ݿ�֧�ֵģ���Ӧ�ȴ�����ЩӦ����������ݿ⣺`python manage.py migrate `
