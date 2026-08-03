# This is a application lifecycle management dir.
1. ConfigMap
2. Secrets
3. InitContainers
4. Sidecar-Containers
# Configmap:
0. Container level
1. This are used to store non-secret env that are node hardcoded in app code but need to cennect to other resource for app to function properly.
2. in this data is used to store key and value, where key is local variable to store actual variables referenced in pod.spec.containers:. 
   - env:
     - name: # name of env var key
     valueFrom:
       configMapKeyRef:
       name: name of configmap
       key: referenced in data key
    it is prefered to store only value with another env var key not referenced in data.
# Secrets
1. Container level
2. Sensitive stuff like db connector password, or user password are store here with base64 encoding, they are not entirely secure but provided basic opacity
3. similar to configmap reference.
  - env:
      name: 
      valueFrom:
        secretKeyRef:
        name: secret config file name
        key: key name that referessecret data
# Init containers
0. Multi-contianer pod setup, hosting, unique containers (more than one type)inside single pod.
1. Used for different case, A initial container used in creating main container and destroyed thereafter.
2. Not in continious use, but only for main caontainer creating helper.
3. example, a contianer desined to make main container wait till backend, and db services are initialsed and waits for designated secs, configured in cmd:[]
4. spec:
     initContainer:
     - name: db-init-container
       image: 
       cmd: ["sleep", "100"]
       restartPolicy: never
     containers:
     - name: main-container
       image: 
       ports:
       - 
# sidecar containers
1. They are not temporary containers.
2. They run along side main container, more like helper container.
3. Real-world example webserver paird with frontend pod.
4. Same syntax as init containers, with restart policy : always