FROM python:2.7

RUN mkdir /srv/redacre
COPY ./ /srv/redacre/api/ 
WORKDIR /srv/redacre/api/

RUN pip install -r requirements.txt

EXPOSE 80
EXPOSE 5000

CMD ["python", "/srv/redacre/api/app.py"]


