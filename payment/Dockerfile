FROM python:3.9
EXPOSE 8080
WORKDIR /opt/server
COPY requirements.txt .
COPY *.py .
COPY payment.ini .
RUN pip3 install -r requirements.txt
CMD ["uwsgi","--ini","payment.ini"]
ENV CART_HOST="cart" \
    CART_PORT=8080 \
    USER_HOST="user" \
    USER_PORT=8080 \
    AMQP_HOST="rabbitmq" \
    AMQP_USER=roboshop \
    AMQP_PASS=roboshop123