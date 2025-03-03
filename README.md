Proyecto API REST - Visits
Este proyecto implementa una API REST en Node.js con Express y PostgreSQL para gestionar visitas. Se despliega en Kubernetes y utiliza Tekton para el pipeline de CI/CD.

🛠 Tecnologías Utilizadas
Backend: Node.js + Express
Base de Datos: PostgreSQL
Contenedores: Docker
Orquestación: Kubernetes (Minikube)
CI/CD: Tekton
📌 Instalación y Configuración
✅ 1. Clonar el Repositorio
bash
Copy
Edit
git clone https://github.com/Crygt/Prueba-CI_CD.git
cd Prueba-CI_CD
✅ 2. Configurar Variables de Entorno
Crear un archivo .env en el directorio raíz con el siguiente contenido:

env
Copy
Edit
DB_HOST=postgres
DB_USER=your_user
DB_PASSWORD=your_password
DB_NAME=visits_db
MODE=develop  # o release
🚀 Construcción y Ejecución Local
✅ 3. Construcción de la Imagen Docker
bash
Copy
Edit
docker build -t crygt/visits-api:latest .
✅ 4. Subir la Imagen a Docker Hub (si aplica)
bash
Copy
Edit
docker login -u crygt -p "your_docker_hub_password"
docker push crygt/visits-api:latest
✅ 5. Ejecutar la API Localmente
bash
Copy
Edit
docker run -p 3001:3001 --env-file .env crygt/visits-api:latest
La API estará accesible en: http://localhost:3001/visits

📦 Despliegue en Kubernetes
✅ 1. Iniciar Minikube
bash
Copy
Edit
minikube start
✅ 2. Aplicar Configuración de Kubernetes
bash
Copy
Edit
kubectl apply -f kubernetes/postgres-deployment.yaml
kubectl apply -f kubernetes/deployment.yaml
kubectl apply -f kubernetes/service.yaml
kubectl apply -f kubernetes/configmap.yaml
kubectl apply -f kubernetes/secret.yaml
✅ 3. Verificar el Estado de los Pods
bash
Copy
Edit
kubectl get pods -A
✅ 4. Acceder a la API desde Minikube
bash
Copy
Edit
minikube service visits-api --url
Esto imprimirá la URL para acceder a la API.

🔗 Pipeline CI/CD con Tekton
✅ 1. Instalar Tekton (si no está instalado)
bash
Copy
Edit
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
✅ 2. Aplicar los Recursos de Tekton
bash
Copy
Edit
kubectl apply -f tekton/tekton-pipeline.yaml
kubectl apply -f tekton/tekton-pipelinerun.yaml
✅ 3. Ejecutar el Pipeline
bash
Copy
Edit
tkn pipeline start build-and-deploy-api -w name=source,claimName=my-pvc -n tekton-unrestricted
✅ 4. Verificar Logs del Pipeline
bash
Copy
Edit
tkn pipelinerun logs -f -n tekton-unrestricted
🏗 Estructura del Proyecto
pgsql
Copy
Edit
Prueba-CI_CD/
├── backend/
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── routes/
│   │   ├── server.js
│   ├── package.json
│   ├── Dockerfile
│   ├── .env
│
├── kubernetes/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│
├── tekton/
│   ├── tekton-pipeline.yaml
│   ├── tekton-pipelinerun.yaml
│
└── README.md
📝 Notas Adicionales
Si usas Minikube, ejecuta:

bash
Copy
Edit
eval $(minikube docker-env)
Para permitir la comunicación con el clúster.

Para ver los logs de la API:

bash
Copy
Edit
kubectl logs -l app=visits-api
🎯 Cómo Probar la API
📌 Endpoint: /visits
Método: GET
Descripción: Devuelve el número de visitas almacenadas en la base de datos.
📌 Ejemplo de Respuesta:

json
Copy
Edit
{
  "visits": 4,
  "mode": "develop"
}
Cada vez que accedas al endpoint /visits, el contador de visitas se incrementará en 1.