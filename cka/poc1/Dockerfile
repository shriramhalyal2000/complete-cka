FROM nginx:alpine AS builder
WORKDIR /app
COPY index.html .

FROM nginx:alpine-slim
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /app /usr/share/nginx/html
RUN chmod -R 755 /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]