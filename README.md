# Text-to-HTML
Covert normal Text to HTML script using Django.

![alt text](https://github.com/New-Byte/flutter/blob/master/text2html.png?raw=true)

## Steps to create Project:
# 1. Create a Django project.
django admin startproject text_to_html
# 2. Create app in that django-project.
python manage.py startapp blog
# 3. Install django-ckeditor, ckeditor provides a text editor in django.
pip install ckeditor
# 4. Add your app name in installed apps.
INSTALLED_APPS = [
    'texteditor',
    'ckeditor',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
# 5. Add files and Folder to the Django Project
We need to create a template folder in the django folder and a urls.py file in the app folder.

1. Create a new folder in the django folder(here, text_to_html folder) save it with the name template.
2. Add the path for this template folder in text_to_html > settings.py .

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR,'templates')],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

3. Create a new file in the app folder(here, blog) save it with the name urls.py .

4. Add path for this url.py file in text_to_html > urls.py .

from django.contrib import admin
from django.urls import path,include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('',include('texteditor.urls'))
]

# 6.  Create Text to Html convertor
For creating text to html converters we have to create a class model named Editor in which we are going to have our text editor template. After the model we need to create a form of that model.
1. Create a model for Editor in blog(your_app_name) > models.py .

from django.db import models
from ckeditor.fields import RichTextField
class Editor(models.Model):
    body=RichTextField(blank=True,null=True)
2. Create a form for the model in texteditor > forms.py .
from django import forms
from .models import Editor
from ckeditor.widgets import CKEditorWidget
class EditorForm(forms.ModelForm):
    body = forms.CharField(widget=CKEditorWidget(),label="Text Editor")
    class Meta:
        model=Editor
        fields="__all__"

3. Create html file in template>index.html .
Use code in the file.

4. Create a path for the index html page in blog > urls.py .
from django.urls import path,include
from . import views
urlpatterns = [
    path('',views.index,name='index')
]

5. Create a function for the index page in blog > views.py .
from django.shortcuts import render
from .models import Editor
from .forms import EditorForm
def index(request):
    form=EditorForm()
    return render(request,'index.html',{'form':form})
