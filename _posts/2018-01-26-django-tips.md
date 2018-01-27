---
title: Django - tips
undefined: django tips tricks
created_date: 2018-01-16 18:30:00 +0000
date: 2018-01-26 13:06:49 +0000
---
1. [Wappalyzer](https://wappalyzer.com/) is an extension which identifies the **valnurabalities on** **website.** In our case, **django**  is detected by the “csrfmiddlewaretoken” whose name we can change, and  whose information we can amplify, by following the subsequent guide [hide-django-from-wappalyzer](https://n3tc4t.github.io/blog/hide-django-from-wappalyzer).
2. Change default admin URL to avoid brute force attacks

```python
# Default admin url
url(r'^admin/', admin.site.urls),

# Replace with following url
url(r'^my_secure_admin/', admin.site.urls),
```

 3. Another way of protecting our **administration panel**  would be to make it accessible only from the network where we have the  server stored, as it must be remembered that this panel is not designed  for end users.
 4. **Check ALLOWED_HOSTS and Debug settings**
 5. use **CRSF always**
 6. **Validate** all the data which we receive in **Django** forms.
 7. HTTPS is must
 8. check security error more here [https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/#run-manage-py-check-deploy](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/#run-manage-py-check-deploy "https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/#run-manage-py-check-deploy")

        python manage.py check --deploy
 9. Use the [orm of django](https://tutorial.djangogirls.org/en/django_orm/) instead of raw whenever possible, and, if necessary to use raw, escape special characters.
10. Another way of checking our **Django website** if we do not have access to our server may be as simple as accessing the following website:[ ponycheckup](https://www.ponycheckup.com/) which will give us a **report** about basic **security problems** and how to improve them.
11. Use the URL tag in templates, so that URLs will not be broken when modified.
```
{% url %}
```
12. Auto register your models in admin page

```python
from django.apps import apps


models = apps.get_models()

for model in models:
admin.site.register(model)
```

1. If you want to curl django login protected APIs use httpie-django-auth

       pip install httpie-django-auth
       
       # now u can use in terminal
       
       http :8000/profile -A=django --auth='username:password'
   1. Handling empty Queryset

      {% for book in books %}
      <li>{{ book }}</li>
      {% empty %}
      <li>Sorry, there are no books.</li>
      {% endfor %}