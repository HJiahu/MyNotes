### ��װdjango
-	��װpip :`python setup.py install`
-	��python��Ŀ¼�µ�script�ļ���д�뻷������
-	���նˣ�`pip install django`

### ��windows�������дdjango����
-	�����ڷ����а�װdjango
-	��װApache��[�ο�](http://www.cnblogs.com/fnng/p/4119712.html)  
-	��װmod_wsgi��[���ص�ַ](http://www.lfd.uci.edu/~gohlke/pythonlibs/#mod_wsgi)
	-	ע�����ص��ļ��ĺ�׺Ϊwhl������׺����Ϊzip��ѹ����ѹ֮�󽫵õ�һ��mod_wsgi.so �ļ������俽����Apache24\modules\ Ŀ¼�¡�
### һЩ����
-	ʹ��`python manage.py check`������磺
-	�������`Segmentation fault`����ִ��`python manage.py check`���������

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
-	Ϊ���ڹ����м���Ӧ�ã���Ҫ�ڹ����ļ��е�settings.py�е�INSTALLED_APPS�����Ӧ�õ����ƣ�`polls.apps.PollsConfig`

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
-	polls/urls.py:

```
from django.conf.urls import url

from . import views

urlpatterns = [
    url(r'^$', views.index, name='index'),
]
```
-	polls/views.py��

```
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```
#### ����server
-	`python manage.py runserver`#Ĭ�϶˿ں�Ϊ��8080��ʹ��localhost:8080��127.0.0.1:8080���ʵ�ǰ��վ
-	` python manage.py runserver 8080`#�޸�Ĭ�϶˿�
-	`python manage.py runserver 0.0.0.0:8000`

### ���ݴ���
-	djangoĬ��ʹ��SQLite��Ϊ���ݿ⣬����ʹ���������ݿ���Ҫ��װ���������ݿ�ӿڲ���setting������Ӧ�����á�ʹ��Ĭ�ϵ�SQLite����Ҫ���κ����á�
-	�ڴ���django����ʱϵͳ��settings.py�е�INSTALLED_APPS�������һЩ�����Ӧ�ã���ЩӦ������Ҫ���ݿ�֧�ֵģ���Ӧ�ȴ�����ЩӦ����������ݿ⣺`python manage.py migrate `
-	�����Լ������ݿ⣺
	-	�༭polls/models.py��

```
class Question(models.Model):
	question_text = models.CharField(max_length = 200)
	pub_date = models.DateTimeField('date published')
	#���һЩ����
	def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)


class Choice(models.Model):
	question = models.ForeignKey(Question,on_delete = models.CASCADE)
	choice_text = models.CharField(max_length = 200)
	votes = models.IntegerField(default = 0)
```
-	ִ��ָ�`python manage.py makemigrations polls `����django������ݿ�����һЩ���ģ����뽫����д�����ݿ⡣ִ������ָ��֮�������һ���ĵ���polls/migrations/0001_initial.py�����ĵ�������django�����ݿ�Ķ�Ӧ������ʹ��python������������
-	ִ��ָ�` python manage.py sqlmigrate polls 0001`ʹdjango���ɶ�Ӧ��sql��䣨���ɵ�������Ϊ���ݿ�Ĳ�ͬ���ı䣩��������䲻�Ǳ���ġ�
-	ִ��ָ�` python manage.py migrate `ִ���������ɵ�sql��䣬ʵ�ֶ����ݿ�ĸ��ġ�
-	**ע��**�����ݿ��еı���Զ��ļ���ǰ׺��`polls_`���������еı����Զ�ת��ΪСд����������django��ʹ�ñ�ʱ����ʹ��models�ж�������ơ�
	-	ϵͳ���Զ�Ϊ��һ������������ԣ����������ϵͳ���Զ�����Ӻ�׺`_id`
-	ͨ��python��django��Ŀ������`python manage.py shell`
-	�������ݿ⣺`from polls.models import Question, Choice`
 
```
>>> from polls.models import Question, Choice   # Import the model classes we just wrote.
>>> Question.objects.all() #û������
[]
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now()) #��Question�д���һ���ĵļ�¼
>>> q.save()  #�����ݿ��б��������¼��
>>> q.id #�����¼�¼��idֵ
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)
>>> q.question_text = "What's up?"  #�ı�ֵ��һ��Ҫ�ٵ���save()���浽���ݿ��С�
>>> q.save()   ################################
>>> Question.objects.all()  #��ʾ���м�¼
[<Question: Question object>]
```
### �û�����
-	���������û���` python manage.py createsuperuser`
