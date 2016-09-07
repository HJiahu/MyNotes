### ����ָ��
-	��װdjango��python>=3.5.1����`pip install django`
-	����project��` django-admin startproject mysite`
-	ʹ������server��������projecrt��`python manage.py runserver`
	-	ָ���˿ںţ�`python manage.py runserver 8080`
	-	ָ��ip��˿ںţ�`python manage.py runserver 0.0.0.0:8000`
-	����app��`python manage.py startapp polls`
-	֪ͨdjango��Ҫ�������ݿ⣺`python manage.py makemigrations polls`
-	�������ڸ������ݿ��SQL��䣺`python manage.py sqlmigrate polls 0001`
-	������д�����ݿ⣺`python manage.py migrate`
-	ͨ������̨�������ݿ⣺` python manage.py shell`
-	����ʹ�õ����ݿ��`from polls.models import Question, Choice`
-	����һ����¼��`q = Question(question_text="What's new?", pub_date=timezone.now())`
	-	Ҫ��ǰ���룺`from django.utils import timezone`
-	�����¼�����ݿ⣺`q.save()`
-	Ϊվ�㴴������Ա��`python manage.py createsuperuser`
-	ʹ��adminҳ����Թ����Ӧ�����ݿ��
	```
	from django.contrib import admin
	from .models import Question

	admin.site.register(Question)
	```

### ��windows�������дdjango����
-	�����ڷ����а�װdjango
-	��װApache��[�ο�](http://www.cnblogs.com/fnng/p/4119712.html)
-	��װmod_wsgi��[���ص�ַ](http://www.lfd.uci.edu/~gohlke/pythonlibs/#mod_wsgi)
	-	ע�����ص��ļ��ĺ�׺Ϊwhl������׺����Ϊzip��ѹ����ѹ֮�󽫵õ�һ��mod_wsgi.so �ļ������俽����Apache24\modules\ Ŀ¼�¡�

### һЩ����
-	ʹ��`python manage.py check`������磺
-	�������`Segmentation fault`����ִ��`python manage.py check`���������

### django��������
-	һ����ͼ����Python��һ�������򷽷���һ�㽫��ͼ�ļ������ƶ�Ϊ��veiws.py��django����urls.py�ļ����ҵ���Ӧ�ĺ�����
-	urls.py��������·��������֮��Ĳ��֣�����ͼ�Ķ�Ӧ��ϵ����django��ÿһ��·������Ӧһ����ͼ��
	-	url()���������ĸ����������������Ǳ���ģ�regex��view��������ѡ�ģ�kwargs��name
		-	regex��һ��urlpatterns���кܶ��url��ϵͳ����ϵ���ѡ��һ��ƥ���url��get��post�������μ�ƥ�䡣
		-	view�����Ӧurl��Ӧ����ͼ��view�ĵ�һ��������HttpResquest�����µ��Ǵ�url����ȡ�Ĳ�����
		-	kwargs���ؼ��ֲ�����
		-	name����url������������Ϳ����������ط���name�������url���ر�����template�У�ʹ��name���Լ��ٺ���ά���Ĺ�������template�У�`<a href="{% url 'detail' question.id %}">{{ question.question_text }}</a>`��urls.py�У�`url(r'^(?P<question_id>[0-9]+)/$', views.detail, name='detail')`���������ڸı�һ��urlʱ��ֻ����urls.py�и��ļ��ɡ�
-	djangoĬ����ÿ��app����ģ��Ŀ¼������polls app����templateĿ¼����staticĿ¼Ҳ��һ����staticĿ¼���ڴ�ž�̬ҳ�棬��css�ļ���` polls/static/polls/style.css`��
-	������settings.py�иı�ʱ����TIME_ZONE = 'Asia/Shanghai'����
-	��project�м���app����setting.py�ļ��е�INSTALED_APPS�����'polls.apps.PollsConfig'��ע�ⶺ��
-	Django adminģ����Ϊ�˷������Ա����޸�ɾ�����ݺ͹���վ��������ġ�admin�����ڹ���Ա������վ�ķ����ߡ�
-	ͬһ��django project�п����ж��app����ôΪ�˱��ͬapp url�е�name��django�ṩ�����ֿռ�ĸ��
	-	��urlpatternsǰ����app_name����������pattern���õ�app��`app_name = 'polls'`
	-	��ģ���ܵ��÷���`<a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a>`

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
