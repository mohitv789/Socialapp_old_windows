FROM nginxinc/nginx-unprivileged:1-alpine
LABEL maintainer="Networking_app"

COPY ./default.conf.tpl /etc/nginx/default.conf.tpl
COPY ./http_proxy.params /etc/nginx/http_proxy.params
COPY ./ws_proxy.params /etc/nginx/ws_proxy.params
COPY ./uwsgi_params /etc/nginx/uwsgi_params
COPY ./run.sh /usr/local/bin/run.sh

ENV LISTEN_PORT=8000
ENV APP_HOST=backend_ea
ENV APP_PORT=9000

USER root

# ensure nginx can read/write the rendered conf
RUN mkdir -p /vol/static \
 && chmod 755 /vol/static \
 && touch /etc/nginx/conf.d/default.conf \
 && chown nginx:nginx /etc/nginx/conf.d/default.conf \
 # fix Windows CRLF and make the script executable
 && sed -i 's/\r$//' /usr/local/bin/run.sh \
 && chmod +x /usr/local/bin/run.sh

VOLUME /vol/static

USER nginx

# Call the script explicitly with sh to avoid shebang edge cases
CMD ["sh", "-c", "/usr/local/bin/run.sh"]
