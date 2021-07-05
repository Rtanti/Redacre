FROM python:2.7

RUN mkdir /srv/redacre
COPY ./ /srv/redacre/api/ 
WORKDIR /srv/redacre/api/
EXPOSE 5000
RUN pip install -r requirements.txt

CMD ["python", "/srv/redacre/api/app.py"]


