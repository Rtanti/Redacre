FROM node

RUN mkdir -p /srv/redacre/frontend
COPY ./ /srv/redacre/frontend/ 
WORKDIR /srv/redacre/frontend/
EXPOSE 8080
EXPOSE 3000
RUN npm install
RUN npm run build
#RUN python app.py 

CMD ["npm", "start"]


