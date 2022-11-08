# Hipster

> Hipster-Shop is a
web-based e-commerce app where users can browse products, add
them to the cart, and purchase them. The application is a widely-
used microservice benchmark designed to aid demonstration and
testing of microservices and cloud-native technologies

## 部署前提
[otel-collector](../otle_jaeger/README.md)

## 部署方式
```
kubectl create ns hipster
kubectl apply -f adservice.yaml
kubectl apply -f cartservice.yaml
kubectl apply -f checkoutservice.yaml
kubectl apply -f currencyservice.yaml
kubectl apply -f emailservice.yaml
kubectl apply -f frontend.yaml
kubectl apply -f paymentservice.yaml
kubectl apply -f productcatalogservice.yaml
kubectl apply -f recommendationservice.yaml
kubectl apply -f redis.yaml
kubectl apply -f shippingservice.yaml
```