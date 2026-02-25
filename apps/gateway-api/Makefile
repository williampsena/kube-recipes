CLUSTER_NAME=gateway-api
ISTIO_VERSION=1.24.3

.PHONY: create delete recreate install install-crds install-istio deploy clean \
        port-forward test pods nodes logs describe _context

# ── Cluster ────────────────────────────────────────────────────────────────────

create:
	kind create cluster --name $(CLUSTER_NAME) --config kind-cluster.yaml

delete:
	kind delete cluster --name $(CLUSTER_NAME)

recreate: delete create

# ── Installation ───────────────────────────────────────────────────────────────

install: _context install-crds install-istio install-prometheus

install-crds: _context
	@echo '📦 Installing Gateway API CRDs'
	@kubectl get crd gateways.gateway.networking.k8s.io &> /dev/null || \
		{ kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.4.0" | kubectl apply -f -; }
	@echo '[OK] Gateway API CRDs ready'

install-istio: _context
	@echo '🚀 Installing Istio $(ISTIO_VERSION)'
	@[ ! -d "istio-$(ISTIO_VERSION)" ] && \
		curl -L https://istio.io/downloadIstio | ISTIO_VERSION=$(ISTIO_VERSION) sh - || true
	@cd istio-$(ISTIO_VERSION) && export PATH=$$PWD/bin:$$PATH && istioctl install \
		--set profile=minimal \
		--set tag=$(ISTIO_VERSION) -y
	@echo '[OK] Istio $(ISTIO_VERSION) installed'

install-prometheus: _context
	@echo '📊 Installing Prometheus'
	@kubectl apply -f prometheus.yaml
	@kubectl wait -n monitoring --for=condition=ready pod \
		-l app=prometheus --timeout=120s
	@echo '[OK] Prometheus ready'

# ── Deploy ─────────────────────────────────────────────────────────────────────

deploy: _context
	@echo '🏗  Creating namespace demo'
	@kubectl create namespace demo --dry-run=client -o yaml | kubectl apply -f -

	@echo '💉 Enabling sidecar injection on namespace demo'
	@kubectl label namespace demo istio-injection=enabled --overwrite

	@echo '🌐 Applying Gateway + HTTPRoute'
	@kubectl apply -f gateway.yaml

	@echo '📦 Applying app manifests'
	@kubectl apply -f app.yaml

	@echo '⏳ Waiting for Gateway to be accepted...'
	@kubectl wait -n demo --for=condition=accepted \
		gateways.gateway.networking.k8s.io demo-gateway --timeout=120s

	@echo '⏳ Waiting for whoami pod to be ready...'
	@kubectl wait -n demo --for=condition=ready pod -l app=whoami --timeout=120s

	@echo '✅ Deploy finished!'

clean: _context
	@echo '🧹 Removing app and gateway resources'
	@kubectl delete -f app.yaml -f gateway.yaml --ignore-not-found
	@kubectl delete namespace demo --ignore-not-found

# ── Observability ──────────────────────────────────────────────────────────────

pods: _context
	@kubectl get pods -n demo -o wide

istio-inspect:
	@kubectl get pods -n istio-system
	@kubectl logs -n istio-system -l app=istiod -f

nodes: _context
	@kubectl get nodes -o wide

logs: _context
	@kubectl logs -n demo -l app=whoami --tail=100 -f

describe: _context
	@kubectl describe pods -l app=whoami -n demo

status: _context
	@echo '--- Pods ---'
	@kubectl get pods -n demo -o wide
	@echo ''
	@echo '--- Gateway ---'
	@kubectl get gateway -n demo
	@echo ''
	@echo '--- HTTPRoute ---'
	@kubectl get httproute -n demo
	@echo ''
	@echo '--- Services ---'
	@kubectl get svc -n demo

# ── Traffic ────────────────────────────────────────────────────────────────────

port-forward: _context
	@echo '🔌 Port-forwarding: localhost:8080 → demo-gateway-istio:80 (namespace demo)'
	@kubectl port-forward svc/demo-gateway-istio 8080:80 -n demo

test:
	@echo '🧪 Testing GET route /'
	@curl -s http://localhost:8080/ | head -20
	@echo ''
	@echo '🧪 Testing headers'
	@curl -s http://localhost:8080/api | jq '.' 2>/dev/null || curl -s http://localhost:8080/api

# ── Helpers ────────────────────────────────────────────────────────────────────

_context:
	@kubectl config use-context kind-$(CLUSTER_NAME) 2>/dev/null || true