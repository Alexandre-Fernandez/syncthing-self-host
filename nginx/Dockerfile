FROM nginx:1.23.2-alpine

ARG CERTIFICATE_HOST
ARG CERTIFICATE_DURATION

RUN apk update \
&& apk add openssl

RUN printf ".\n.\n.\n.\n.\n${CERTIFICATE_HOST}\n.\n" \
| openssl req -x509 -nodes -days ${CERTIFICATE_DURATION} -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt -subj "/" \
&& openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048

COPY snippets/self-signed.conf /etc/nginx/snippets/self-signed.conf
COPY snippets/ssl-params.conf /etc/nginx/snippets/ssl-params.conf

RUN mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.bak
COPY conf.d/default.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
EXPOSE 443