# Tarea3

# Ejemplo de Proyecto Kubernetes

Este proyecto muestra cómo desplegar una API y una base de datos MongoDB en diferentes namespaces en Kubernetes.

## Requisitos

- Tener instalado `kubectl` y `docker`.
- Acceso a un clúster de Kubernetes.
- Un registro de Docker local en `localhost:5000` en ejecución.

## Pasos para ejecutar el proyecto

1. **Crear los namespaces**:
- kubectl create namespace api-namespace
- kubectl create namespace db-namespace

2. **Construir las imágenes de Docker**:
- docker build -t imagenapi .
- docker build -t imagendb .

3. **Etiquetar las imágenes de Docker**:
- docker tag imagenapi localhost:5000/imagenapi
- docker tag imagendb localhost:5000/imagendb

4. **Subir las imágenes al registro local**:
- docker push localhost:5000/imagenapi
- docker push localhost:5000/imagendb

5. **Aplicar los archivos de Kubernetes**:
- kubectl apply -f api-deployment.yaml -n api-namespace
- kubectl apply -f api-service.yaml -n api-namespace
- kubectl apply -f db-deployment.yaml -n db-namespace
- kubectl apply -f db-service.yaml -n db-namespace

7. **Reenviar el puerto local a la API**:
- Identificar el nombre del pod de la API con `kubectl get pods -n api-namespace`.
- Ejecutar `kubectl port-forward <nombre-del-pod-api>  8080:80 -n api-namespace` para reenviar el puerto local  8080 al puerto  80 del pod.

8. **Consumir la API**:
- Acceder a la API utilizando `localhost` y el puerto local que especificaste (en este caso,  8080):
- curl http://localhost:8080/api/data
