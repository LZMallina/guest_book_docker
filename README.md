# IBM DevOp Certificate: Introduction to Containers with Docker, Kubernetes, and OpenShift final project

## Steps

* Write commands on Dockerfile to build and dpush image to path: guestbook/v1/guestbook/Dockerfile

* Export the namespace as an environment variable

$ export MY_NAMESPACE=sn-labs-$USERNAME

* Build the guestbook app using the Docker Build command

$ docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1

* Push the image to IBM cloud Container Registry

$ docker push us.icr.io/$MY_NAMESPACE/guestbook:v1

* Verify that the image was pushed successfully

$ ibmcloud cr images

* update deployment.yml file

spec:
      containers:
      - image: us.icr.io/MY_NAMESPACE/guestbook:v1
        imagePullPolicy: Always
        name: guestbook

* Apply the deployment using: 

$ kubectl apply -f deployment.yml

* Open a new Terminal and enter the  command:

$ kubectl port-forward deployment.apps/guestbook 3000:3000

$ kubectl get pods for trouble-shooting if there's deployment issue
## Autoscale the guestbook application using horizontal pod autoscaler

1. Autoscale the Guestbook deployment using 

$ kubectl autoscale deployment guestbook --cpu-percent=5 --min=1 --max=10

2. Check the current status of the newly-made HorizontalPodAutoscaler by running:

$ kubectl get hpa guestbook

3. Open another new terminal and put 

$ kubectl run -i --tty load-generator --rm --image=busybox:1.36.0 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- https://lzkrishmom-3000.theiaopenshift-0-labs-prod-theiaopenshift-4-tor01.proxy.cognitiveclass.ai/
/env /info; done"

4. Go back to the first terminal and run command to observe the replicas increase in accordance with autoscaling:

$ kubectl get hpa guestbook --watch

$ ctrl+C to stop kubernetes watch

5. Check the status of the newly-made HorizontalPodAutoscaler by running:

$ kubectl get hpa guestbook

## Perform rolling updates and rollbacks on the Guestbook application

1. Update the title and header in index.html to <title>Lili Zhao Guestbook - v2</title>

</head>

<body>
  <div id="header">
    <h1>Lili Zhao Guestbook - v2</h1>

2. Run the below command to build and push your updated app image:

$ docker build . -t us.icr.io/$MY_NAMESPACE/guestbook:v1 && docker push us.icr.io/$MY_NAMESPACE/guestbook:v1

3. Update the values of the CPU in the deployment.yml to cpu: 5m and cpu: 2m

4. Apply the changes to the deployment.yml file

$ kubectl apply -f deployment.yml

5. Open a new terminal and run the port-forward command again to start the app:

$ kubectl port-forward deployment.apps/guestbook 3000:3000

6. run the below command to see the history of deployment rollouts: 

$ kubectl rollout history deployment/guestbook

7. Run the below command to see details of Revision of the deployment rollout:

$ kubectl rollout history deployments guestbook --revision=2

8. Run the below command to get the replica sets and observe the deployment which is being used now:

$ kubectl get rs

9. Run the below command to undo the deployment and set it to revision 1:

$ kubectl rollout undo deployment/guestbook --to-revision=1

10. Run the below command to get the replica sets after Rollout has been undone.  

$ kubectl get rs

