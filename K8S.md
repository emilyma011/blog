kubectl get pod -n nacos-server
kubectl get svc -n istio-system
kubectl logs -f -n test gateway343435 -c gateway
kubectl get gateways.networking.istio.io -n test
kubectl get gateways.networking.istio.io -n test gateway -o yaml

kubectl get pod -n test --show-lables


