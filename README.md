# django-helm

1. Build the docker image:
`cd image; sudo docker build . -t django-web`

2. Create a namespace for the django deployment

3. Install the helm chart:
`helm install -n django django .`

4. With django installed on your machine with PIP. Create a new project:
`django-admin startproject mysite`

5. Copy the manage.py and mysite folder to your kubernetes PVC that was created. This is mounted into /usr/src/app inside the python container.

6. Edit your settings.py with something like this for postgresql support. You should be able to get the generated secret from secrets in Kubernetes when the postgresql database was created:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'djangodb',
        'USER': 'postgres',
        'PASSWORD': 'mypassword',
        'HOST': 'django-postgresql',
        'PORT': '5432',
    }
} 
```

7. Initilize the database by executing to the django pod:
`python manage.py migrate`

8. You should now be able to access the django web interface. By default it is exposed via a NodePort with this helm chart.
