# Run DjangoCustomUserStarter-codespaces in Codespaces

## Resources

* Get environment variables:
  * `printenv`
* [`django-browser-reload` - PyPI](https://pypi.org/project/django-browser-reload/)

## Questions

* How to restrict the number of packages installed in the container?

## Errors and Issues

### Linting Issues

* <https://github.com/orgs/community/discussions/46885>

### Integrated Browser Doesn't Load

* Error:

  * Integrated browser doesn't load:
      ![Webpage doesn't load in integrated browser](../local_things/images/browser_not_loading.png)
  * Browser error message:

      ```text
      brucestull-bookish-succotash-xvx9j7qpr9g3vj7q-8000.preview.app.github.dev refused to connect.
      ```

      ![Browser error message "brucestull-bookish-succotash-xvx9j7qpr9g3vj7q-8000.preview.app.github.dev refused to connect."](../local_things/images/refused_to_connect.png)

* Solution:

  * Add a value for `X_FRAME_OPTIONS` in [`config/settings/development.py`](../config/settings/development.py):
  * [Django: X_FRAME_OPTIONS](https://docs.djangoproject.com/en/4.1/ref/settings/#x-frame-options)

      ```python
      X_FRAME_OPTIONS = "ALLOW-FROM preview.app.github.dev"
      ```

### Login in Forwarded Browser Gets CSRF Error

* Error:

  * Forwarded browser doesn't load:
  
      ![Webpage doesn't load in forwarded browser](../local_things/images/csrf_error.png)
  
  * Error text:
  
      ```text
      Forbidden (403)
      CSRF verification failed. Request aborted.
      
      Help
      Reason given for failure:
      
          Origin checking failed - https://brucestull-bookish-succotash-xvx9j7qpr9g3vj7q-8000.preview.app.github.dev does not match any trusted origins.
          
      In general, this can occur when there is a genuine Cross Site Request Forgery, or when Django’s CSRF mechanism has not been used correctly. For POST forms, you need to ensure:
      
      Your browser is accepting cookies.
      The view function passes a request to the template’s render method.
      In the template, there is a {% csrf_token %} template tag inside each POST form that targets an internal URL.
      If you are not using CsrfViewMiddleware, then you must use csrf_protect on any views that use the csrf_token template tag, as well as those that accept the POST data.
      The form has a valid CSRF token. After logging in in another browser tab or hitting the back button after a login, you may need to reload the page with the form, because the token is rotated after a login.
      You’re seeing the help section of this page because you have DEBUG = True in your Django settings file. Change that to False, and only the initial error message will be displayed.
      
      You can customize this page using the CSRF_FAILURE_VIEW setting.
      ```
  
* Solution:
  * Modify [`config/settings/common.py`](../config/settings/common.py):

    ```python
    import os

    if 'CODESPACE_NAME' in os.environ:
        codespace_name = os.getenv("CODESPACE_NAME")
        codespace_domain = os.getenv("GITHUB_CODESPACES_PORT_FORWARDING_DOMAIN")
        CSRF_TRUSTED_ORIGINS = [f'https://{codespace_name}-8000.{codespace_domain}']
    ```

### `pip list` Populates with More Packages Than Expected

* `pip list`:

  ```bash
  @brucestull ➜ /workspaces/DjangoCustomUserStarter-codespaces (main) $ pip list
  Package                  Version
  ------------------------ -----------
  anyio                    3.6.2
  argon2-cffi              21.3.0
  argon2-cffi-bindings     21.2.0
  arrow                    1.2.3
  asgiref                  3.6.0
  asttokens                2.2.1
  attrs                    22.2.0
  Babel                    2.11.0
  backcall                 0.2.0
  beautifulsoup4           4.11.1
  bleach                   6.0.0
  certifi                  2022.12.7
  cffi                     1.15.1
  charset-normalizer       3.0.1
  colorama                 0.4.6
  comm                     0.1.2
  contourpy                1.0.7
  cycler                   0.11.0
  debugpy                  1.6.6
  decorator                5.1.1
  defusedxml               0.7.1
  Django                   4.1.5
  django-browser-reload    1.6.0
  docutils                 0.19
  entrypoints              0.4
  executing                1.2.0
  fastjsonschema           2.16.2
  fonttools                4.38.0
  fqdn                     1.5.1
  gitdb                    4.0.10
  GitPython                3.1.30
  gunicorn                 20.1.0
  idna                     3.4
  ipykernel                6.20.2
  ipython                  8.8.0
  ipython-genutils         0.2.0
  isoduration              20.11.0
  jedi                     0.18.2
  Jinja2                   3.1.2
  joblib                   1.2.0
  json5                    0.9.11
  jsonpointer              2.3
  jsonschema               4.17.3
  jupyter_client           7.4.9
  jupyter_core             5.1.5
  jupyter-events           0.6.3
  jupyter_server           2.1.0
  jupyter-server-mathjax   0.2.6
  jupyter_server_terminals 0.4.4
  jupyterlab               3.5.3
  jupyterlab-git           0.41.0
  jupyterlab-pygments      0.2.2
  jupyterlab_server        2.19.0
  kiwisolver               1.4.4
  MarkupSafe               2.1.2
  matplotlib               3.6.3
  matplotlib-inline        0.1.6
  mistune                  2.0.4
  nbclassic                0.4.8
  nbclient                 0.7.2
  nbconvert                7.2.9
  nbdime                   3.1.1
  nbformat                 5.7.3
  nest-asyncio             1.5.6
  notebook                 6.5.2
  notebook_shim            0.2.2
  numpy                    1.24.1
  nvidia-cublas-cu11       11.10.3.66
  nvidia-cuda-nvrtc-cu11   11.7.99
  nvidia-cuda-runtime-cu11 11.7.99
  nvidia-cudnn-cu11        8.5.0.96
  packaging                23.0
  pandas                   1.5.3
  pandocfilters            1.5.0
  parso                    0.8.3
  pexpect                  4.8.0
  pickleshare              0.7.5
  Pillow                   9.4.0
  pip                      22.3.1
  platformdirs             2.6.2
  plotly                   5.13.0
  prometheus-client        0.16.0
  prompt-toolkit           3.0.36
  psutil                   5.9.4
  psycopg2                 2.9.5
  ptyprocess               0.7.0
  pure-eval                0.2.2
  pycparser                2.21
  Pygments                 2.14.0
  pyparsing                3.0.9
  pyrsistent               0.19.3
  python-dateutil          2.8.2
  python-json-logger       2.0.4
  pytz                     2022.7.1
  PyYAML                   6.0
  pyzmq                    25.0.0
  requests                 2.28.2
  rfc3339-validator        0.1.4
  rfc3986-validator        0.1.1
  scikit-learn             1.2.1
  scipy                    1.10.0
  seaborn                  0.12.2
  Send2Trash               1.8.0
  setuptools               58.1.0
  six                      1.16.0
  smmap                    5.0.0
  sniffio                  1.3.0
  soupsieve                2.3.2.post1
  sqlparse                 0.4.3
  stack-data               0.6.2
  tenacity                 8.1.0
  terminado                0.17.1
  threadpoolctl            3.1.0
  tinycss2                 1.2.1
  tomli                    2.0.1
  torch                    1.13.1
  tornado                  6.2
  traitlets                5.8.1
  typing_extensions        4.4.0
  tzdata                   2022.7
  uri-template             1.2.0
  urllib3                  1.26.14
  wcwidth                  0.2.6
  webcolors                1.12
  webencodings             0.5.1
  websocket-client         1.4.2
  wheel                    0.38.4
  whitenoise               6.3.0

  [notice] A new release of pip available: 22.3.1 -> 23.0
  [notice] To update, run: python -m pip install --upgrade pip
  @brucestull ➜ /workspaces/DjangoCustomUserStarter-codespaces (main) $
  ```

* Resource:
  * [Introduction to dev containers - docs.github.com](https://docs.github.com/en/codespaces/setting-up-your-project-for-codespaces/adding-a-dev-container-configuration/introduction-to-dev-containers)
