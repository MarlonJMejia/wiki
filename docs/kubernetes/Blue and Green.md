# How to deploy a blue green deployment and expose it to the web

??? example "Example Config"

        apiVersion: v1
    kind: ConfigMap
    metadata:
    name: index-html-blue
    namespace: default
    data:
    index.html: |
        <!DOCTYPE html>
            <html>
            <head>
            <style>
            body {
            background-color: lightblue;
            }
            </style>
            </head>
            <body>
            <p>This page has a light blue background color!</p>

            </body>
            </html>
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: index-html-green
    namespace: default
    data:
    index.html: |
        <!DOCTYPE html>
            <html>
            <head>
            <style>
            body {
            background-color: lightgreen;
            }
            </style>
            </head>
            <body>

            <p>This page has a light green background color!</p>

            </body>
            </html>
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: index-html-yellow
    namespace: default
    data:
    index.html: |
        <!DOCTYPE html>
            <html>
            <head>
            <style>
            body {
            background-color: lightblue;
            }
            </style>
            </head>
            <body> 
            <p>Canary V1</p>
            </body>
            </html>
    ---
    apiVersion: v1
    kind: ConfigMap
    metadata:
    name: index-html-yellow-v2
    namespace: default
    data:
    index.html: |
        <!DOCTYPE html>
            <html>
            <head>
            <style>
            body {
            background-color: lightblue;
            }
            </style>
            </head>
            <body> 
            <p>Canary V2</p>
            </body>
            </html>
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: nginx-deployment-blue
    namespace: default
    spec:
    selector:
        matchLabels:
        app: nginx-blue
    replicas: 2
    template:
        metadata:
        labels:
            app: nginx-blue
        spec:
        containers:
        - name: nginx
            image: nginx:latest
            ports:
            - containerPort: 80
            volumeMounts:
                - name: nginx-index-file
                mountPath: /usr/share/nginx/html/
        volumes:
        - name: nginx-index-file
            configMap:
            name: index-html-blue
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: nginx-deployment-green
    namespace: default
    spec:
    selector:
        matchLabels:
        app: nginx-green
    replicas: 2
    template:
        metadata:
        labels:
            app: nginx-green
        spec:
        containers:
        - name: nginx
            image: nginx:latest
            ports:
            - containerPort: 80
            volumeMounts:
                - name: nginx-index-file
                mountPath: /usr/share/nginx/html/
        volumes:
        - name: nginx-index-file
            configMap:
            name: index-html-green
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: nginx-deployment-canary
    namespace: default
    spec:
    selector:
        matchLabels:
        app: web
        version: v1
    replicas: 2
    template:
        metadata:
        labels:
            app: web
            version: v1
        spec:
        containers:
        - name: nginx
            securityContext:
            allowPrivilegeEscalation: false
            image: nginx:latest
            ports:
            - containerPort: 80
            volumeMounts:
                - name: nginx-index-file
                mountPath: /usr/share/nginx/html/
        volumes:
        - name: nginx-index-file
            configMap:
            name: index-html-yellow
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: nginx-deployment-canary-v2
    namespace: default
    spec:
    selector:
        matchLabels:
        app: web
        version: v2
    replicas: 2
    template:
        metadata:
        labels:
            app: web
            version: v2
        spec:
        containers:
        - name: nginx
            securityContext:
            allowPrivilegeEscalation: false
            image: nginx:latest
            ports:
            - containerPort: 80
            volumeMounts:
                - name: nginx-index-file
                mountPath: /usr/share/nginx/html/
        volumes:
        - name: nginx-index-file
            configMap:
            name: index-html-yellow-v2

# Updating