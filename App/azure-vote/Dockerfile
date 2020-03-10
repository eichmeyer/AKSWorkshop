FROM    tiangolo/meinheld-gunicorn-flask

RUN     apt-get update && \
        apt-get install default-libmysqlclient-dev -y && \
        pip install flask-mysql && \
        pip install cryptography
 
ADD     /azure-vote /app