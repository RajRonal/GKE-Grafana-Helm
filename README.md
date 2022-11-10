# GKE-Grafana-Helm
Scrapping GO- custom metrics using Prometheus and Grafana,,installed within GKE cluster i.e Helm for k8s package manager

Step 1: Create A GKE/ACR/ECR repository to save your docker image
Step 2: Create A GKE/EKS/AKS to deploy your app in a managed kubernetes cluster.
      2(a): Step to Create a GKE Cluster
          # gcloud init ( To initialize  a gcloud project with automatic authentication)
          # gcloud container clusters create (name of your cluster) --num-nodes=(required num of nodes in each az) --enable-autoscaling  --min-nodes (num)   --max-nodes (num)
          # gcloud container clusters get-credentials (cluster name)  (To configure kube config)
      2(b) Install Helm (If you haven't)
      2(c) Steps to Configure Prometheus and  Grafana using Helm
          # helm repo add prometheus-community https://prometheus-community.github.io/helm-charts (Define public Kubernetes chart repository in the Helm configuration)
          # helm repo update (Update local repositories)
          # kubectl create ns prometheus (Create a namespace for Prometheus and Grafana resources)
          # helm install prometheus prometheus-community/kube-prometheus-stack -n prometheus (Install Prometheus using HELM)
          # kubectl get all -n prometheus (Check all resources in Prometheus Namespace)
          # kubectl edit svc prometheus-kube-prometheus-prometheus -n prometheus ( Here we are changing from ClusterIP to LoadBalancer/NodePort)
          # kubectl edit svc prometheus-grafana - n prometheus ( Here we are changing from ClusterIP to LoadBalancer/NodePort)
       2(d) Create  a Service Monitor to Scrape your Custom metrics in Prometheus
           (check template-> deployment.yaml(bottom)
       2(e) Install your templates using helm install [][]
       2(f) Create a custom_values.yaml to override the ScrapeConfig  file of prometheus (Check custom_values.yaml for reference)
       2(g) helm upgrade   --namespace prometheus   -f custom_values.yaml   prometheus prometheus-community/kube-prometheus-stack
            (Run the above command to upgrade  helm chart and  add custom Scrap Endpoint in prometheus)
            # kubectl edit svc prometheus-kube-prometheus-prometheus -n prometheus ( Here we are changing from ClusterIP to LoadBalancer/NodePort)
            # kubectl edit svc prometheus-grafana - n prometheus ( Here we are changing from ClusterIP to LoadBalancer/NodePort)
            (Since the helm chart is upgraded the Loadbalancer will be reconfigured to default value(ClusterIp)
        
        
Points:-
  # Auto-Scaling group should be  enabled in cluster in order to use prometheus
  # If target labels are dropped,then might be a mismatch in ServiceMonitor and Service labels and selectors
          
            
            

        

          

