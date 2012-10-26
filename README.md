# Radmin the totally rad django admin console.

## Install & Config
Make sure the ```django.template.loaders.app_directories.Loader``` is enabled

Ex:
```
TEMPLATE_LOADERS = (
	'django.template.loaders.filesystem.Loader',
	'django.template.loaders.app_directories.Loader',
)
```

Add radmin to your installed apps. Make sure you place it before the django admin

Ex:
```
INSTALLED_APPS = (
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.sites',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'radmin', # !!! This needs to come before django.contrib.admin
    'django.contrib.admin',
    'myapp',
)
```

Add radmin urls to you projects urls.py

Ex:

```
urlpatterns = patterns('',
    url(r'^radmin/', include('radmin.urls')), #NEED
    url(r'^admin/', include(admin.site.urls)),
)
```

## Usage Examples
Register radmin items in your apps admin.py 

Ex:
```
from myapp.models import Thing
from radmin import console

admin.site.register(Thing)

# ----------------------------------------------radmin starts here-------------------------------------
# shows up everwhere
console.register_to_all('Everywhere Action', 'myapp.views.current_datetime', True) 

# shows up only at /admin
console.register_to_admin_index('Admin Index Action', 'myapp.views.current_datetime', True) 

#shows up at app index /admin/app
console.register_to_app('MyApp', 'App level action', 'myapp.views.current_datetime') 

#shows up when you are at the model list eg /admin/app/model
console.register_to_model_list(Thing, 'Model List Level Action','myapp.views.current_datetime') 

# shows up at single model /admin/app/model/id. The call back should expect to recieve the model id back
console.register_to_model(Thing, 'Model Level Action', 'myapp.views.get_current_model_name') 
```
