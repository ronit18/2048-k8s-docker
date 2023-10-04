# 2048

A simple HTML CSS JS game copied from
https://github.com/gabrielecirulli/2048
https://hczhcz.github.io/2048/20ez/

And dockerized + deployed on kubernetes for fun.

-   Dockerfile

```Dockerfile
FROM nginx

COPY . /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

```

-   k8s-manifest.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: deployment-2048
spec:
    selector:
        matchLabels:
            app: deployment-2048
    template:
        metadata:
            labels:
                app: deployment-2048
        spec:
            containers:
                - name: deployment-2048
                  image: ronitgandhi/deployment-2048:latest
                  ports:
                      - containerPort: 80

---
apiVersion: v1
kind: Service
metadata:
    name: svc-2048
spec:
    type: NodePort
    selector:
        app: deployment-2048
    ports:
        - port: 80
          targetPort: 80
          nodePort: 30007
```
