[]([text](https://bhu786.github.io/Kubernetes-YAML-Master-Guide/))


# ---------------- NAMESPACE ----------------
apiVersion: v1
kind: Namespace
metadata:
  name: demo-app

---
# ---------------- CONFIGMAP ----------------
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: demo-app
data:
  APP_ENV: production
  APP_NAME: my-app

---
# ---------------- SECRET ----------------
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
  namespace: demo-app
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=   # base64

---
# ---------------- PVC ----------------
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-pvc
  namespace: demo-app
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi

---
# ---------------- DEPLOYMENT ----------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-deployment
  namespace: demo-app
  labels:
    app: my-app

spec:
  replicas: 2

  selector:
    matchLabels:
      app: my-app

  template:
    metadata:
      labels:
        app: my-app

    spec:

      # ---------------- NODE SELECTOR ----------------
      nodeSelector:
        disktype: ssd

      # ---------------- INIT CONTAINER ----------------
      initContainers:
      - name: init-check
        image: busybox
        command: ["sh", "-c", "echo Waiting for app..."]

      containers:
      - name: app-container
        image: nginx:latest

        ports:
        - containerPort: 80

        # ---------------- ENV FROM CONFIGMAP ----------------
        env:
        - name: APP_ENV
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: APP_ENV

        # ---------------- ENV FROM SECRET ----------------
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_PASSWORD

        # ---------------- RESOURCES ----------------
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "500m"
            memory: "256Mi"

        # ---------------- LIVENESS PROBE ----------------
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 10
          periodSeconds: 5

        # ---------------- READINESS PROBE ----------------
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 5
          periodSeconds: 3

        # ---------------- VOLUME MOUNT ----------------
        volumeMounts:
        - name: app-storage
          mountPath: /usr/share/nginx/html

      # ---------------- VOLUMES ----------------
      volumes:
      - name: app-storage
        persistentVolumeClaim:
          claimName: app-pvc

---
# ---------------- SERVICE ----------------
apiVersion: v1
kind: Service
metadata:
  name: app-service
  namespace: demo-app
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 80
  type: ClusterIP

---
# ---------------- INGRESS ----------------
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
  namespace: demo-app
spec:
  rules:
  - host: myapp.local
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: app-service
            port:
              number: 80

---
# ---------------- HPA ----------------
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: app-hpa
  namespace: demo-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: app-deployment
  minReplicas: 2
  maxReplicas: 5
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50