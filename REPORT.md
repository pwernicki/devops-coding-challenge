# Your report goes here ....
Install k3s
curl -sfL https://get.k3s.io | sh -

Install kustomize (optional)
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh" | bash
sudo mv kustomize /usr/local/bin/

Build images and push them to docker hub
kustomize build build/ | kubectl apply -f -

Deploy app to k8s
kustomize build app/ | kubectl apply -f -

We can use kubectl without kustomize and apply manifest one by one or create one big yaml

1. **CI/CD**
   - How would you set up a basic CI/CD pipeline to build, test, and deploy the microservices to the remote Kubernetes cluster?
        - I will use github actions pipeline for build images. Then kustomize with dev, staging and prod overlays. Combine this with ArgoCD for deployment.  
   - What deployment strategy would you recommend for deploying updates to the microservices in the Kubernetes cluster, and how would you implement it in the CI/CD pipeline?
        - Definetely we need to create new images with updates and promote new tag to deployment. I will do it with github actions but in separete branch from main.
        - Now we must promote the new tag to staging env. This can be done by update tag in yaml files by pipeline. ArgoCD will recognize change and do the rest for us.
        - If everything goes well we can do promotion to production.

2. **Metrics**:
   - What tools or platforms do you use for collecting and visualizing metrics in a Kubernetes environment?
        - I will use Prometheus and Grafana as a solution.
   - What are some key metrics you would monitor for a Kubernetes cluster and why?
        - For sure kubernetes core components like: kubelet, etcd, api server. Kubernetes won't work without those.
        - We should consider monitoring of cpu/mem even if we always set resources/limits.

3. **Logging**:
   - Describe your approach to centralized logging in a Kubernetes environment.
        - I will use fluentd as a sidecar to pod and elesticsearch for centralization.
   - What tools do you prefer for log aggregation and analysis, and why?
        - To be honest fluentd and elastisearch is only one I know.

4 **Monitoring**:
    - What is your process for setting up monitoring alerts in Kubernetes?
        - We can setup some alerts in prometheus.
    - How do you ensure high availability and fault tolerance in your monitoring setup?
        - We can setup HA for prometheus but also we should consider use external services like pingdom for crucial alerts.
