## Let's deploy to Kubernetes!

First, are we connected to the local Kubernetes cluster?

```bash
kubectl config current-context
# docker-desktop
```

Yay!

If so, let's try running the service we built in exercise 1:

```bash
kubectl create deployment hello-docker --image=senorflor/hello-docker
```

```bash
kubectl get deployments
# NAME           READY   UP-TO-DATE   AVAILABLE   AGE
# hello-docker   0/1     1            0           111s
```

Interesting... unavailable... what does that mean, and why would that be?

<details><summary>See answer</summary>
<p>

Let's look at the things the deployment is managing: the pods.

```bash
kubectl get pods
# NAME                            READY   STATUS             RESTARTS   AGE
# hello-docker-65b959d9f7-gqcsj   0/1     CrashLoopBackOff   2          41s
```
That's right: this is not a server, it's just a simple container that will echo something then exit. Since we are creating a server deployment, Kubernetes interprets this as the pod crashing. Abort abort!

(If you want to just run a one-time or periodic job on your cluster, you can do it with the appropriately named [Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/))

```bash
kubectl delete deployment hello-docker
```

Let's create a deployment with a server instead:

```bash
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

```bash
 kubectl get deployments
# NAME         READY   UP-TO-DATE   AVAILABLE   AGE
# hello-node   1/1     1            1           6s
```

Let's look at the pods on this one too!

```bash
kubectl get pods
# NAME                          READY   STATUS    RESTARTS   AGE
# hello-node-55b49fb9f8-pnqt6   1/1     Running   0          21s
```

That's more like it!

</p>
</details>

#### Let's expose the server as a failure-proof service!

OK, that's cool, there's a server running in my cluster, and Kubernetes will manage and keep it up for me. How can I make it a bit more failure tolerant, and then expose it to the world?

##### Scaling up

```bash
kubectl scale deployment hello-node --replicas=2
# deployment.extensions/hello-node scaled
```

OK! Let's check on that:

```bash
kubectl get deployments
# NAME         READY   UP-TO-DATE   AVAILABLE   AGE
# hello-node   2/2     2            2           5m
```

```bash
kubectl get pods
# NAME                          READY   STATUS    RESTARTS   AGE
# hello-node-55b49fb9f8-lw58f   1/1     Running   0          11s
# hello-node-55b49fb9f8-pnqt6   1/1     Running   0          5m5s
```

And then there were two.

##### Saying hello to others

```bash
kubectl expose deployment hello-node --type=NodePort --name=say-hello --port 8080
# service/say-hello exposed
```

```bash
kubectl get services
# NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
# kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          46h
# say-hello    NodePort    10.106.129.162   <none>        8080:31636/TCP   3s
```

That port, `31636` in the above example output, is the important part! In a browser, or using curl, hit `localhost:<PORT>`

```
Hello World!
```

Success! Congratulations, you're now a container wrangler level 1.