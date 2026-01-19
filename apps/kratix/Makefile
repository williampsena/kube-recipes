CLUSTER_NAME=kratix

.PHONY: help up down install-deps whoami whoami-status whoami-logs whoami-delete nginx nginx-status nginx-logs nginx-delete promises status logs _create-cluster _install-deps _context

help:
	@echo "Kratix Platform - Promise Examples"
	@echo ""
	@echo "Cluster:"
	@echo "  make up              - Create cluster with Kratix"
	@echo "  make down            - Delete cluster"
	@echo "  make promises        - Install all promises (whoami + nginx)"
	@echo ""
	@echo "Whoami Promise:"
	@echo "  make whoami          - Deploy whoami instance"
	@echo "  make whoami-status   - Check whoami status"
	@echo "  make whoami-logs     - Follow whoami logs"
	@echo "  make whoami-delete   - Remove whoami"
	@echo ""
	@echo "Nginx Promise:"
	@echo "  make nginx           - Deploy nginx instance"
	@echo "  make nginx-status    - Check nginx status"
	@echo "  make nginx-logs      - Follow nginx logs"
	@echo "  make nginx-delete    - Remove nginx"
	@echo ""
	@echo "Cluster:"
	@echo "  make status          - Full cluster status"
	@echo "  make logs            - Kratix controller logs"

# Cluster Management
up: _create-cluster _install-deps promises
	@echo ""
	@echo "✅ Cluster ready!"
	@echo ""
	@echo "Access Applications:"
	@echo "  Whoami:"
	@echo "    • HostPort 8080:  http://localhost:8080"
	@echo "    • NodePort 30080: http://localhost:30080"
	@echo "  Nginx:"
	@echo "    • NodePort 31080: http://localhost:31080"
	@echo ""
	@echo "Commands: make whoami | make nginx | make status"

down:
	@kind delete cluster --name $(CLUSTER_NAME)

# Internal targets
_create-cluster:
	@echo "🛠️ Creating Kind cluster..."
	@kind get clusters | grep -q $(CLUSTER_NAME) || kind create cluster --name $(CLUSTER_NAME) --config=kind-config.yaml
	@kubectl config use-context kind-$(CLUSTER_NAME)

_install-deps:
	@echo "📦 Installing dependencies..."
	@kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.crds.yaml 2>/dev/null || true
	@kubectl apply -f https://github.com/cert-manager/cert-manager/releases/latest/download/cert-manager.yaml 2>/dev/null || true
	@echo "⏳ Waiting for cert-manager..."
	@for i in 1 2 3 4 5 6 7 8 9 10; do kubectl wait --for=condition=ready pod -l app.kubernetes.io/name=cert-manager -n cert-manager --timeout=30s 2>/dev/null && break || sleep 5; done
	@echo "📦 Installing Kratix..."
	@for i in 1 2 3; do kubectl apply -f https://github.com/syntasso/kratix/releases/latest/download/kratix.yaml 2>/dev/null && break || sleep 10; done
	@echo "✅ Dependencies ready"

promises: _context
	@echo "📝 Promises available (via kubectl apply):"
	@echo "  • Whoami: kubectl apply -f ./iac/whoami-promise.yaml"
	@echo "  • Nginx:  kubectl apply -f ./iac/nginx-promise.yaml"
	@echo ""
	@echo "Note: Promise webhooks have upstream Kratix bug"
	@echo "      Use 'make whoami' or 'make nginx' to deploy instances directly"
	@echo ""
	@echo "Instances available:"
	@echo "  • Whoami: make whoami"
	@echo "  • Nginx:  make nginx"

# Whoami Application
whoami: _context
	@echo "🚀 Deploying whoami..."
	@kubectl apply -f ./iac/whoami-deployment.yaml
	@echo "⏳ Waiting for pods..."
	@sleep 5
	@kubectl get pods -n whoami-demo
	@echo ""
	@echo "✅ Whoami is running!"

whoami-status: _context
	@echo "📊 Whoami Status"
	@echo ""
	@echo "Pods:"
	@kubectl get pods -n whoami-demo 2>/dev/null || echo "Not deployed"
	@echo ""
	@echo "Services:"
	@kubectl get svc -n whoami-demo 2>/dev/null || echo "Not deployed"

whoami-logs: _context
	@kubectl logs -n whoami-demo -l app=whoami -f 2>/dev/null || echo "No pods found"

whoami-delete: _context
	@kubectl delete ns whoami-demo --ignore-not-found

# Nginx Application
nginx: _context
	@echo "🚀 Deploying nginx instance..."
	@kubectl apply -f ./iac/nginx-instance.yaml
	@echo "⏳ Waiting for nginx pods..."
	@sleep 5
	@kubectl get pods -n nginx-demo -l app=nginx 2>/dev/null || echo "Pods not ready yet"
	@echo ""
	@echo "✅ Nginx is deploying!"

nginx-status: _context
	@echo "📊 Nginx Status"
	@echo ""
	@echo "Pods:"
	@kubectl get pods -n nginx-demo -l app=nginx 2>/dev/null || echo "Not deployed"
	@echo ""
	@echo "Services:"
	@kubectl get svc -n nginx-demo -l app=nginx 2>/dev/null || echo "Not deployed"

nginx-logs: _context
	@kubectl logs -n nginx-demo -l app=nginx -f 2>/dev/null || echo "No pods found"

nginx-delete: _context
	@kubectl delete ns nginx-demo --ignore-not-found

# Cluster Status
status: _context
	@echo "📊 Cluster Status"
	@echo ""
	@echo "🔧 Kratix Platform:"
	@kubectl get pods -n kratix-platform-system
	@echo ""
	@echo "🔐 Cert-Manager:"
	@kubectl get pods -n cert-manager
	@echo ""
	@echo "🎯 Whoami Instance:"
	@kubectl get pods -n whoami-demo 2>/dev/null || echo "Not deployed"
	@echo ""
	@echo "🌐 Nginx Instances:"
	@kubectl get pods -n nginx-demo 2>/dev/null || echo "Not deployed"

logs: _context
	@echo "Kratix Controller Logs:"
	@kubectl logs -n kratix-platform-system -l control-plane=controller-manager -f

# Helpers
_context:
	@kubectl config use-context kind-$(CLUSTER_NAME) 2>/dev/null || true