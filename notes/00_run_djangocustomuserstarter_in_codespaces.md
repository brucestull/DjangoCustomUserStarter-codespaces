# Run DjangoCustomUserStarter-codespaces in Codespaces

## Resources

* Get environment variables:
  * `printenv`
* [`django-browser-reload` - PyPI](https://pypi.org/project/django-browser-reload/)

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
