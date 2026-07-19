# Iniciar o projeto Django

```
python -m venv venv
. venv/bin/activate
pip install django
django-admin startproject project .
python manage.py startapp contact
```

# Configurar o git

```
git config --global user.name 'Seu nome'
git config --global user.email 'seu_email@gmail.com'
git config --global init.defaultBranch main
# Configure o .gitignore
git init
git add .
git commit -m 'Mensagem'
git remote add origin URL_DO_GIT
```

# Migrando a base de dados do Django

```
python manage.py makemigrations
python manage.py migrate
```

# Criando e modificando a senha de um super usuário Django

```
python manage.py createsuperuser
python manage.py changepassword USERNAME
```

# Trabalhando com o model do Django

```python
# Importe o módulo
from contact.models import Contact
# Cria um contato (Lazy)
# Retorna o contato
contact = Contact(**fields)
contact.save()
# Cria um contato (Não lazy)
# Retorna o contato
contact = Contact.objects.create(**fields)
# Seleciona um contato com id 10
# Retorna o contato
contact = Contact.objects.get(pk=10)
# Edita um contato
# Retorna o contato
contact.field_name1 = 'Novo valor 1'
contact.field_name2 = 'Novo valor 2'
contact.save()
# Apaga um contato
# Depende da base de dados, geralmente retorna o número
# de valores manipulados na base de dados
contact.delete()
# Seleciona todos os contatos ordenando por id DESC
# Retorna QuerySet[]
contacts = Contact.objects.all().order_by('-id')
# Seleciona contatos usando filtros
# Retorna QuerySet[]
contacts = Contact.objects.filter(**filters).order_by('-id')
```
# Criando uma visualização para ver os contatos individualmente com detalhes

## Primeira coisa criar um template
 ### contact/templates/contact/contact.html
```html
{% extends "global/base.html" %}

{% block content %}
    <div class="single-contact">
        <h1 class="single-contact-name">
            {{contact.first_name}} {{contact.last_name}}
        </h1>

        <p><b>ID: </b>{{contact.id}}</p>
        <p><b>E-mail: </b>{{contact.email}}</p>
        <p><b>Phone: </b>{{contact.phone}}</p>
        <p><b>Created Date: </b>{{contact.created_date}}</p>
        <p><b>Description: </b>{{contact.description}}</p>
        <p><b>Category: </b>{{contact.category}}</p>
    </div>
{% endblock content %}
```
## Segunda coisa ajustar a view, criando uma nova view no template
 ### contact/views/contact_views.py
```python
def contact(request, contact_id):
    # single_contact = Contact.objects.filter(pk=contact_id).first()
    single_contact = get_object_or_404(Contact, pk=contact_id, show=True)

    context = {"contact": single_contact}

    return render(request, "contact/contact.html", context)
``` 
## Terceira coisa ajustar a url, criando uma nova url no template
### contact/urls.py 
> path("<int:contact_id>/", views.contact, name="contact")
```python
urlpatterns = [
    path("", views.index, name="index"),
    path("<int:contact_id>/", views.contact, name="contact"),
]
``` 
## Quarta coisa ajustar index.html, criando urls clicáveis para abrir o novo template
### contact/templates/contact/index.html 
```html
    <td class="table-cel">
        <a 
            class="table-link"
            href="{% url "contact:contact" contact.id %}" 
        >
            {{ contact.id }}
        </a>
    </td>
``` 

