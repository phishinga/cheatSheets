#  Kubectl

### To list all nodes and their IP addresses in an AKS cluster

    $kubectl get nodes -o wide

### List all containers from all namespaces and all pods 

    $kubectl get pods --all-namespaces -o jsonpath='{range .items[*].spec.containers[*]}{.name}{"\n"}{end}'
 
### List all pods from all namespaces and their containers (shell)

    $kubectl get pods --all-namespaces -o jsonpath='{range .items[*]}{"\n"}{"NAMESPACE: "}{.metadata.namespace}{"\n"}{"POD NAME: "}{.metadata.name}{"\n"}{"CONTAINER NAMES:"}{range .spec.containers[*]}{"\n"}{.name}{end}{end}'

### List all pods from all namespaces and their containers (PowerShell)

    kubectl get pods --all-namespaces -o=jsonpath="{range .items[*]}{.metadata.namespace}{','}{range .spec.containers[*]}{.image}{','}{end}{end}"
    
### Execute a command in a running container in a Kubernetes pod

    $kubectl exec -i -t -n azure-vote azure-vote-back-797b558855-zv2mf -c azure-vote-back -- sh

### Root container?

    $kubectl exec -n azure-vote azure-vote-back-797b558855-zv2mf -c azure-vote-back -- sh -c "id"

- More advanced scripts here: https://github.com/phishinga/scripts
