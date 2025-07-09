# Kubernets

# Kind Installation 
    Download the kind Binary 
        curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64

    Make It Executable
        chmod +x ./kind

    Move It to a PATH
        sudo mv ./kind /usr/local/bin/kind

    Verify Installation 
        kind version

# Kubectl instllation
    sudo snap install kubectl --classic

    kubectl version --client



# Creating cluster
    1. single-node cluster
          kind create cluster

    2. default config 
        kind create cluster --name mycluster 

    3. Custom config (multi-node) 
        # cluster-config.yaml
        kind: Cluster
        apiVersion: kind.x-k8s.io/v1alpha4
        nodes:
        - role: control-plane
        - role: worker
        - role: worker

        kind create cluster --name multi-node --config cluster-config.yaml 

    4. 
        kind create cluster --image version  (for example : --image kindest/node:v1.28.0)




# Verifying the Cluster
    kubectl get nodes

# Cluster info
    kubectl cluster-info

# Cluster version
    kubectl version

# Listing Clusters
    kind get clusters

# Deleting a cluster
    kind delete cluster
            or 
    kind delete cluster --name <cluster_name>






kubectl get nodes -o wide

# Communicate to the cluster

    1. Create the Pod in Imperative way 
        kubectl run <your-pod-name> --image <image-name> --port <port-name>
            
            EX: kubectl run nginx-pod --image nginx:latest --port 80

        a) Status of the pod
            kubectl get pods

        b) Details about the pod 
            kubectl describe pod <your-pod-name>
                EX: kubectl describe pod nginx-pod  

        c) Get the logs 
            kubectl logs nginx-pod        

        d) Access the Nginx web page from our local browser 
            kubectl port-forward pod/<your-pod-name> <hostport:pod port>
            
            EX: kubectl port-forward pod/nginx-pod 8080:80

            To run this in background : kubectl port-forward pod/ngnx-pod 8080:80 &

        e) Delete the pod 
            kubectl delete pod <podname>

    2. Declarative way 
        a) Create a yaml file 
            apiVersion: v1
            kind: Pod
            metadata:
                name: nginx-pod-declarative # Name of your pod
                labels:
                app: nginx-declarative      # Labels 
            spec:
                containers:
                - name: nginx-container       # Name of the container within the Pod
                image: nginx:latest         # The Docker image to use
                ports:
                - containerPort: 80         # The port the application inside the container listens on     

        b) Apply the yaml file to the cluster
            kubectl apply -f <file-name>
                EX: kubectl apply -f declarative.yaml



# Multinode cluster 

    create a configuration file 
        EX:multinode_cluster.yaml

    kind create cluster --config <file_name> --name multicluster
        EX : kind create cluster --config multinode_cluster.yaml --name multicluster


    check node status 
        kubectl get nodes 


# Replication controller

    creat configuration file 
        replication_controller.yaml 

        kubectl apply -f replication_controller.yaml

    check status 
        kubectl get rc 

    describe 
        kubectl describe rc

    Scalling Replication controller
        a) kubectl scale rc <replication_name> --replicas=5
        b) usign yaml file 


# Replicaset 

    creat configuration file 
        replica_set.yaml

        kubectl apply -f <file_name>
            EX: kubectl apply -f replica_set.yaml

    check status 
        kubectl get rs 


# Deployment
    A Kubernetes Deployment automates the management of Pods via ReplicaSets. It provides rolling updates, rollback capability, and ensures high availability by maintaining the desired number of Pods during updates.


    Create the yaml file for deploymnt 
        deployment.yaml

        kubectl apply -f deployment.
        
    check status
        kubectl get deployment 
        kubectl get deployment <deplyment_name>


    delete 
        kubectl delete -f deployment.yaml

    scale up 
        kubectl scale deployment <deployment_name> --replicas=5 


# Services   

    Service is a fundamental concept that provides a stable network endpoint for a set of Pods. It acts as an abstraction layer, allowing other applications or external clients to reliably access your applications running in Pods, even as those Pods are created, destroyed, and have their IP addresses changed.

    1.ClusterIP

        Create configuration for deployment file 
            service_deployment.yaml file 
            kubectl apply -f service_deployment.yaml
        
        Create configuration for cluster ip service file 
            service_clusterip.yaml file 
            kubectl apply -f service_clusterip.yaml

        Verify the status 
            kubectl get deployment 
            kubectl get service  

        Create a test pod to access the service 
            kubectl run test-client --rm -it --image=busybox --restart=Never -- sh 
            in the coommand line 
             wget -qO- http://<service_name>
             EX :     wget -qO- http://service-name 

    
    2.NodePort 

        Create configuration for deployment file 
            service_nodeport_deployment.yaml
            kubectl apply -f service_nodeport_deployment.yaml
        
        Create configuration for cluster ip service file 
            service_nodeport.yaml
            kubectl apply -f service_nodeport.yaml 


        To access in web(using pport forward )
            kubectl port-forward <service_name> 8080:80
            kubectl port-forward service/nginx-service 8080:80


# Namespaces 

    Is like a virtual cluster inside your actual cluster.
    It’s a logical separation used to organize and manage resources like pods, services, deployments, etc.

    EX : 
        Production
        Development
        Instead of running everything in the same namespace (default), you create:
            kubectl create namespace prod
            kubectl create namespace dev

        Now, you can deploy the same app in both:
            kubectl apply -f myapp.yaml -n prod
            kubectl apply -f myapp.yaml -n dev

    Verify status 
        kubectl get ns

        kubectl describe namespace <name>
        EX:  kubectl describe namespace default

    Creating Namespace 
        1. Imperative way            
            kubectl create namespace my-namespace 
        2. Declarative way
            create yaml file 
                    apiVersion: v1
                    kind: Namespace
                    metadata:
                    name: my-namespace

    Creating resourses using namespaces
        1. 
            kubectl create deployment nginx-deploy --image=nginx -n <namespce_name>
            EX : kubectl create deployment nginx-deploy --image=nginx -n default 
          
            kubectl get pods -n default


        2. create yaml file 
            kubectl apply -f namespace.yaml   

