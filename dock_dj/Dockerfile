FROM python:3.13-rc-slim

RUN mkdir docker
WORKDIR docker

ADD requirements.txt /docker/

RUN pip install --upgrade pip
RUN pip install -r requirements.txt

ADD . /docker/


CMD python manage.py migrate \
    && python manage.py shell -c "from django.contrib.auth import get_user_model; User = get_user_model(); User.objects.filter(username='root').exists() or User.objects.create_superuser('root', 'root@example.com', 'root')" \
    && python manage.py collectstatic --no-input \
    && gunicorn prediction.wsgi:application --bind 0.0.0.0:8000 --log-level info

