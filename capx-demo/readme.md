export EXP_CLUSTER_RESOURCE_SET=true 
clusterctl init -i nutanix

export TEST_CLUSTER_NAME=mytestcluster1
export TEST_NAMESPACE=mytestnamespace
clusterctl generate cluster ${TEST_CLUSTER_NAME} \
    -i nutanix \
    --target-namespace ${TEST_NAMESPACE}  \
    --kubernetes-version v1.22.9 \
    --control-plane-machine-count 1 \
    --worker-machine-count 3 > ./cluster.yaml
kubectl create ns ${TEST_NAMESPACE}
kubectl apply -f ./cluster.yaml -n ${TEST_NAMESPACE}

clusterctl get kubeconfig ${TEST_CLUSTER_NAME} -n ${TEST_NAMESPACE} > ${TEST_CLUSTER_NAME}.kubeconfig
kubectl --kubeconfig ./${TEST_CLUSTER_NAME}.kubeconfig get nodes
