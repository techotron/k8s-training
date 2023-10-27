CLUSTER_NAME=kind-ckad
CONTEXT=kind-${CLUSTER_NAME}

.PHONY: up
up:
	kind create cluster --name ${CLUSTER_NAME} --config kind-cluster-config.yaml && \
	$(MAKE) deploy-nginx-ingress && \
	sleep 90 && \
	$(MAKE) setup-cluster

.PHONY: down
down:
	kind delete cluster --name ${CLUSTER_NAME}


.PHONY: deploy-nginx-ingress
deploy-nginx-ingress:
	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml --context ${CONTEXT}

.PHONY: setup-cluster
setup-cluster:
	kubectl apply -f cluster-resources.yaml --context $(CONTEXT)
