### PROJECT SET UP 

## Django set up 

    1. Crearte virtual environment and activate it 
        python3 -m venv venv 
        source ven/bin/activate 

    2. Install Django 
        pip install django 
        Create requirements file add django to it 

    3. Create Django Project 
        django-admin startproject k8django
        cd k8django 

    4. Create app and add end points to it 
        python3 manage.py startapp app 
    
    5. Add app into project settings file 
        add app in installed apps 
        set ALLOWED_HOSTS = ['*']


    6. Add app urls configurations into project files 

    7. Run migrations 
        python3 manage.py makemigrations
        python3 manage.py migrate

    8. Test project is running or not 
        python3 manage.py runserver 
        visit http://127.0.0.1:8000/ OR http://localhost:8000/ 


## Dockerize Django Project 

    1. In the root folder add Dockerfile and commands to it 

    2. Build Docker image 
        docker build -t k8django .

    3. Run docker image 
        docker run -d -p 8000:8000 --name k8django-container k8django
        to run in different port 
        docker run -d -p 8001:8000 --name k8django-container k8django

        a) -d runs the container in detached mode.

        b) -p 8000:8000 maps host port 8000 to the container port 8000 (as used in CMD).

        c) --name k8django-container gives container a name.

        d) k8django is the image built.


## Deployment usign Kubernetes 

    1. Build Docker image for django application 
        docker build -t k8django .

        To check images : docker images 

    2. Create Kind Cluster 
        kind create cluster --name k8django-cluster 

        To check cluster : kind get clusters 

    3. Load the Image into Kind(For local deployment) 
        docker build -t k8django:latest .
        kind load docker-image <image_name> --name <cluster_name>
            EX : kind load docker-image k8django:latest --name k8django-cluster
        

    3. Kubernetes Manifests
        create deployment.yaml file 
        create service.yaml file  

    4. Deploy to Kubernetes 
        kubectl apply -f deployment.yaml
        kubectl apply -f service.yaml

        kubectl get deployment
        kubectl get deployment <deplyment_name>
        kubectl delete deployment <service_name>

        kubectl get service
        kubectl get service <service_name>          
        kubectl delete service <service_name>


    5. Check pods 
        kubectl get pods 
        kubectl describe pod <pod-name>
        kubectl delete pods --all

    6. Access the App 
        a) with port forwarding 
            kubectl port-forward <service_name> 8000:8000

            EX : kubectl port-forward service/k8django-service 8000:8000
            http://localhost:8000/
        




