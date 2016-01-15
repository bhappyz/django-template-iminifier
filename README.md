django-template-minifier
========================

Django application, providing simple template loader. It reduces HTML output in templates by stripping out whitespace characters between HTML and django template tags.

Things to note:
* It **does not** make any fancy compression, to do that use [GZip Middleware](https://docs.djangoproject.com/en/dev/ref/middleware/#module-django.middleware.gzip).
* To compress CSS and JS use [django-compressor](https://github.com/jezdez/django_compressor).


Installation
-----------

* via [virtualenv](http://www.virtualenv.org/en/latest/#what-it-does) - yup we highly recommend it!

```bash
pip install django-template-minifier
```

* or [download 1.0 package](https://github.com/iRynek/django-template-minifier/archive/v1.0.zip), unzip and run:

```bash
python setup.py install
```

* or [download 1.0 package](https://github.com/iRynek/django-template-minifier/archive/v1.0.zip), unzip and copy template_minifier directory to Your ``PYTHONPATH``


Basic usage
-----------

Modify Your Django project settings's module.

**For production** (note cached loader):

```python
TEMPLATE_LOADERS = (('django.template.loaders.cached.Loader', (
        'template_minifier.template.loaders.filesystem.Loader',
        'template_minifier.template.loaders.app_directories.Loader',
    )),
)
```

**For development** (each refresh reloads template):

```python
TEMPLATE_LOADERS = (
    'template_minifier.template.loaders.filesystem.Loader',
    'template_minifier.template.loaders.app_directories.Loader',
)
```

Be happy having less spaces and new lines in Your templates!


Advanced usage:
-----------

Using modified settings You can:
* turn off stripping spaces between HTML tags

```python
TEMPLATE_MINIFIER_HTML_TAGS = False # default = True
```

* turn off stripping for specific templates

```python
SETTINGS_TEMPLATE_MINIFIER_EXCLUDE = ('marketing_email.html', 'lead_email.html') 
```

* turn off stripping spaces between Django template tags (\s{%, %}\s)

```python
TEMPLATE_MINIFIER_TEMPLATE_TAGS = False # default = True
```

* turn off all stripping

```python
TEMPLATE_MINIFIER = False # default = True
```

* run Your own strip_function, which preprocess templates

```python
TEMPLATE_MINIFER_STRIP_FUNCTION = 'template_minifier.utils.strip_spaces_in_template'
```

(There is a typo in variable name, see #2 for details)

* **use only in production**

```python
if DEBUG:
    TEMPLATE_MINIFIER = False
```

Known issues:
-----------
* Don't use // one line comments in Your inline javascript &lt;script&gt; or .js templates. In some cases,
if You are using lot of {% if %} there, it can comment out }; or }, for example:

```js
// comment something - !!it's evil!!
{% if %}
function name(){
}
{% endif %}
```

**Use /* */ instead**

```js
/* comment something - it's nice and clean <3! */
{% if %}
function name(){
}
{% endif %}
```

Or just set TEMPLATE_MINIFIER_TEMPLATE_TAGS = False


To do:
-----------
* Tests!
* {% new_line %} template_tag
