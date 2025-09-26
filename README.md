# Como resolver o problema da pasta static no deploy

Para que o sistema carregue os arquivos da pasta static no deploy, siga os passos abaixo:

1. Abra o arquivo `settings.py`
2. Procure por `ALLOWED_HOSTS` e acrescente o localhost para fazer o teste na máquina local após o deploy. Vai ficar assim:
~~~python
ALLOWED_HOSTS = [
    'localhost',
    'seu-site.onrender.com'
]
~~~
3. Faça a instalação do pacote whitenoise na venv pelo terminal: `pip install whitenoise`
4. Procure por `MIDDLEWARE` e adicione `"whitenoise.middleware.WhiteNoiseMiddleware",` após `'django.middleware.security.SecurityMiddleware',`. Ficará assim:
~~~python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    "whitenoise.middleware.WhiteNoiseMiddleware",
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
~~~
5. Procure pela linha de comando `STATIC_URL = 'static/'`, e após ela, acrescente `STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')`
6. Procure por `STATICFILES_DIRS`, e adicione `os.path.join(STATIC_ROOT, ''),`. Ficará assim:
~~~python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
    os.path.join(STATIC_ROOT, ''),
]
~~~
7. No final do código-fonte, acrescente os códigos:
~~~python
WHITENOISE_USE_FINDERS = True
WHITENOISE_AUTOREFRESH = DEBUG
~~~
8. Empacote os arquivos para servir em produção digitando o seguinte comando no terminal: `python manage.py collecticstatic`. Ele irá criar uma nova pasta chamada `staticfiles` na raíz do projeto
9. Gere novamente o arquivo `requirements.txt` com `pip freeze > requirements.txt`
10. Faça o envio de novo com `git add .`, depois `git commit -m "Sua mensagem."`, e finalize com `git push`
11. Espere o render fazer o deploy, e ao terminar, acesse o site.