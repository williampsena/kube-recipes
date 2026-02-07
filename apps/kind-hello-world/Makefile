CLUSTER_NAME=kind-lab

.PHONY: create delete recreate pods nodes deploy port-forward clean

create:
	kind create cluster --name $(CLUSTER_NAME) --config kind-cluster.yaml

delete:
	kind delete cluster --name $(CLUSTER_NAME)

recreate: delete create

nodes: _context
	kubectl get nodes -o wide

describe: _context
	kubectl describe pods whoami -n default

pods: _context
	kubectl get pods -o wide

deploy: _context
	kubectl apply -f whoami.yaml

port-forward: _context
	kubectl port-forward pod/whoami 8080:80

clean: _context
	kubectl delete -f whoami.yaml

_context:
	@kubectl config use-context kind-$(CLUSTER_NAME) 2>/dev/null || true