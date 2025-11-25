# ---------- Build ----------
FROM python:3.9 AS build
WORKDIR /opt/server
COPY requirements.txt .
RUN pip3 install --user -r requirements.txt
COPY payment.ini .
COPY *.py .

# ---------- Runtime ----------
FROM python:3.9-slim
EXPOSE 8080
WORKDIR /opt/server

# Create non-root user
RUN groupadd -r roboshop && \
    useradd -r -g roboshop roboshop && \
    chown -R roboshop:roboshop /opt/server

# Copy python dependencies + app
COPY --from=build /root/.local /root/.local
COPY --from=build /opt/server /opt/server

# Add pip installed bin path
ENV PATH="/root/.local/bin:${PATH}"

# App env
ENV CART_HOST="cart" \
    CART_PORT="8080" \
    USER_HOST="user" \
    USER_PORT="8080" \
    AMQP_HOST="rabbitmq" \
    AMQP_USER="roboshop" \
    AMQP_PASS="roboshop123"

USER roboshop
CMD ["uwsgi","--ini","payment.ini"]