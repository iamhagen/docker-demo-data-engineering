# Docker und Kubernetes Vorlesung

Dieses Projekt dient dazu, Docker und Kubernetes vorzustellen. Im Repository befindet sich ein Node-Server mit einem Next.js-Blog über Kubernetes und Docker.

## Aufgabenstellung

1. **Repository klonen**: Klone das Repository auf deinen Computer.

   ```bash
   git clone https://inf-git.th-rosenheim.de/studhagejo5992/docker-demo-data-engineering.git
   cd docker-demo-data-engineering
   ```
2. **Dockerfile verstehen**: Analysiere das Dockerfile, um zu verstehen, wie das Docker-Image für den Blog erstellt wird.
3. **Docker-Image erstellen**: Erstelle ein Docker-Image für den Blog.

   ```bash
   docker build -t docker-demo-data-engineering .
   ```
4. **Docker-Container erstellen**: Erstelle einen Docker-Container und leite den Port 3000 weiter.

   ```bash
   docker run -p 3000:3000 docker-demo-data-engineering
   ```
5. **Blog öffnen**: Öffne den Blog auf deinem Rechner unter [http://localhost:3000](http://localhost:3000).
6. **Kubernetes Cluster mit Docker**: Führe den Blog als Kubernetes-Cluster auf deinem Computer aus.

   - Installiere Docker: [Docker Installation](https://docs.docker.com/get-docker/)
   - Starte Docker Desktop und aktiviere Kubernetes in den Einstellungen.
   - Erstelle ein Kubernetes-Deployment für den Blog:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: blog-deployment
     spec:
       replicas: 1
       selector:
         matchLabels:
           app: blog
       template:
         metadata:
           labels:
             app: blog
         spec:
           containers:
           - name: blog
             image: docker-demo-data-engineering:latest
             ports:
             - containerPort: 3000
     ```
   - Erstelle das Deployment:
     ```bash
     kubectl apply -f deployment.yaml
     ```
   - Erstelle einen Service, um den Blog zugänglich zu machen:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: blog-service
     spec:
       type: NodePort
       selector:
         app: docker-demo-data-engineering
       ports:
         - protocol: TCP
           port: 80
           targetPort: 3000
           nodePort: 30000
     ```
   - Erstelle den Service:
     ```bash
     kubectl apply -f service.yaml
     ```
   - Öffne den Blog im Browser:
     ```bash
     kubectl port-forward service/blog-service 3000:3000
     ```

## Hinweise

Dieses Projekt verwendet [Tailwind CSS](https://tailwindcss.com) [(v3.0)](https://tailwindcss.com/blog/tailwindcss-v3).
