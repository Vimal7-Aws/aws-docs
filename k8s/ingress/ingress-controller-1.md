
In this Kubernetes ingress tutorial, you will learn the **basic concepts of ingress**, the native ingress resource object, and the **concepts involved in ingress controllers**.

Kubernetes Ingress is a resource to add rules to route traffic from external sources to the applications running in the kubernetes cluster.

Here are the topics that I will cover in this complete guide to Kubernetes Ingress:

> **Note**: Write note
> 
What is Kubernetes Ingress?
---------------------------

The literal meaning: I**ngress** refers to the act of entering.

It is the same in the Kubernetes world as well. Ingress means the traffic that enters the cluster and egress is the traffic that exits the cluster.

Ingress is a native Kubernetes resource like pods, deployments, etc. Using ingress, you can **maintain the DNS routing configurations**. The ingress controller does the actual routing by reading the routing rules from ingress objects stored in etcd.

Let's understand ingress with a high-level example.

Without Kubernetes ingress, to expose an application to the outside world, you will add a service Type Loadbalancer to the deployments. Here is how it looks. (I have shown the nodePort just to show the traffic flow)

In the same implementation, with ingress, there is a reverse proxy layer (Ingress controller implementation) between the load balancer and the kubernetes service endpoint.

Here is a very high-level view of ingress implementation. In later sections, we will see a detailed architecture covering all the key concepts.

> **Note:** The AWS, GCP cloud ingress controller implementation differs a little. For example AWS loadbalancer itself acts as a ingress controller. Refer to the GKE ingress setup blog to understand more about GCP loadbalancer.

Before Kubernetes Ingress?
--------------------------

Before the Kubernetes Ingress was stable, a custom Nginx or an HAproxy kubernetes deployment would be exposed as a Loadbalancer service for routing external traffic to the internal cluster services.

The routing rules are added as a configmap in the Nginx/HAProxy pods. Whenever there is a change in dns or a new route entry to be added, it gets updated in the configmap, and pod configs are reloaded, or it gets re-deployed.

Kubernetes ingress also follows a similar pattern by having the routing rules maintained as native Kubernetes ingress objects instead of a configmap.

And in place of Nginx/HAProxy, we have ingress controllers, a customized version of Nginx/HAProxy, etc., which fetches the routing rules dynamically.

Also, there were implementations using consul and other service discovery tools to update DNS changes to Nginx or HAproxy without downtime, which brings the exact implementation as ingress.

When it comes to openshift, the router (HAproxy implementations) concept made it easy to expose service endpoints outside the cluster. All you have to do is make a router config (Openshift YAML object), and the openshift router takes care of everything. It is similar to Kubernetes ingress.

How Does Kubernetes Ingress work?
---------------------------------

If you are a beginner and trying to understand ingress, there is possible confusion on how it works.

For example, You might ask, hey, I created the ingress rules, but I am not sure how to map it to a domain name or route the external traffic to internal deployments.

You need to be very clear about two key concepts to understand that.

1.  **Kubernetes Ingress Resource:** Kubernetes ingress resource is responsible for storing DNS routing rules in the cluster.
2.  **Kubernetes Ingress Controller:** Kubernetes ingress controllers (Nginx/HAProxy etc.) are responsible for routing by accessing the DNS rules applied through ingress resources.

Let's look at both the ingress resource and ingress controller in detail.

Kubernetes Ingress Resource
---------------------------

The Kubernetes Ingress resource is a native kubernetes resource where you specify the DNS routing rules. This means, that you map the external DNS traffic to the internal Kubernetes service endpoints.

It requires an ingress controller for routing the rules specified in the ingress object. Let's have a look at a very basic ingress resource.

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
name: test-ingress
namespace: dev
spec:
rules:
- host: test.apps.example.com
http:
paths:
- backend:
serviceName: hello-service
servicePort: 80
```

The above declaration means, that all calls to `test.apps.example.com` should hit the service named `hello-service` residing in the dev namespace.

As you can see, all it has is routing rules. You can add multiple routing endpoints for path-based routing, you can add TLS configuration, etc.

Key things to understand about ingress objects.

1.  An ingress object requires an ingress controller for routing traffic.
2.  And most importantly, the external traffic does not hit the ingress API, instead, it will hit the ingress controller service endpoint configured directly with a load balancer.

Now, let's understand the ingress controller.

Kubernetes Ingress Controller
-----------------------------

Ingress controller is **not a native Kubernetes implementation**. This means It doesn't come default in the cluster.

We need to set up an ingress controller for the ingress rules to work. There are several open-source and enterprise ingress controllers available.

An ingress controller is typically a reverse web proxy server implementation in the cluster. In kubernetes terms, it is a reverse proxy server deployed as kubernetes deployment exposed to a service type Loadbalancer.

You can have multiple ingress controllers in a cluster mapped to multiple load balancers. Each ingress controller should have a unique identifier named **ingress-class** added to the annotation.

How Does an Ingress Controller Work?
------------------------------------

Nginx is one of the widely used ingress controllers.

So let's take an example of Nginx ingress controller implementation to understand how it works.

1.  The `nginx.conf` file inside the Nginx controller pod is a lua template that can talk to **Kubernetes ingress API** and get the latest values for traffic routing in real-time. Here is the template file.
2.  The Nginx controller talks to Kubernetes ingress API to check if there is any rule created for traffic routing.
3.  If it finds any ingress rules, the Nginx controller generates a routing configuration inside `/etc/nginx/conf.d` location inside each nginx pod.
4.  For each ingress resource you create, Nginx generates a configuration inside `/etc/nginx/conf.d` location.
5.  The main `/etc/nginx/nginx.conf` file contains all the configurations from `etc/nginx/conf.d.`
6.  If you update the ingress object with new configurations, the Nginx config gets updated again and does a graceful reload of the configuration.

If you connect to the Nginx ingress controller pod using exec and check the `/etc/nginx/nginx.conf` file, you can see all the rules specified in the ingress object applied in the conf file.

Ingress & Ingress Controller Architecture
-----------------------------------------

Here is the architecture diagram that explains the ingress & ingress controller setup on a kubernetes cluster.

It shows ingress rules routing traffic to two `payment` & `auth` applications

Now if you look at the architecture, it will make more sense and you will probably be able to understand how each ingress workflow works.

Click to View in HD

List of Kubernetes Ingress Controller
-------------------------------------

Following are the **commonly used ingress controllers** available for Kubernetes.

1.  Nginx Ingress Controller (Community & From Nginx Inc)
2.  Traefik
3.  HAproxy
4.  Contour
5.  GKE Ingress Controller for GKE
6.  AWS ALB Ingress Controller Fro AKS
7.  Azure Application Gateway Ingress Controller

Learnk8s has created a great document comparing all the available ingress controllers. Take a look at the comparison document.

Deploy Your First Ingress Controller
------------------------------------

To understand the ingress and ingress controller better, you should get your hands dirty with an implementation.

I have published a detailed guide on setting up an Nginx ingress controller.

I have also covered some essential concepts like ingress admission controllers in detail.

Also, I have given step-by-step manifest deployment and explanation to get a deep understanding.

Check out the Nginx ingress controller setup guide.

Kubernetes Ingress FAQs
-----------------------

### Is Ingress a load balancer?

Ingress is not a load balancer. It contains all the routing rules, custom headers, and TLS configurations. The ingress controller acts as a load balancer.

### Why do I need an ingress controller?

The ingress controller is responsible for the actual routing of external traffic to kubernetes service endpoints. Without an ingress controller, the routing rules added to the ingress will not work.

### What is the difference between ingress and Nginx?

Ingress is a kubernetes object. Nginx is used as an ingress controller (Reverse proxy).

### Can we route traffic to multiple paths using ingress?

Yes. With a single ingress definition, you can add multiple path-based routing configurations.

### Does ingress support TLS configuration?

Yes. You can have TLS configurations in your ingress object definition. The TLS certification will be added as a Kubernetes secret and referred to in the ingress object.

Conclusion
----------

In this **Kubernetes Ingress Tutorial**, we have seen how ingress works in Kubernetes. and its associated components. Choosing an ingress controller for production depends on various factors and requirements.

You can use the ingress controller comparison document as a reference to choose an ingress controller.

Maybe just started learning about ingress or currently using ingress in projects.

Either way, drop a comment below and let me know your thoughts.

The advanced implementation of ingress is Gateway API. Please read our detailed Gateway API tutorial for beginners to know more.

Also, if you are learning Kubernetes, check out my 30+ comprehensive Kubernetes tutorials.

Kubernetes

Share Share Share Share Email Copy

About the author

Bibin Wilson
------------

Bibin Wilson (authored over 300 tech tutorials) is a cloud and DevOps consultant with over 12+ years of IT experience. He has extensive hands-on experience with public cloud platforms and Kubernetes.

Featured

### Kubernetes Built-in AI/ML Features You Should Know

05 Jun 2025 ### Python For DevOps: Guide for DevOps Engineers

17 May 2025 ### Kubernetes Tutorial For Beginners: 65 Comprehensive Guides

07 Jan 2025 ### Jenkins Tutorial For Beginners: 21+ Practical Guides

01 Apr 2023

Latest

### Authenticate Azure CLI Using Service Principal (Tutorial)

03 Jun 2025 ### Kubernetes Init Containers: A Complete Guide

03 Jun 2025 ### How To Setup and Configures Proxy Server - Squid Proxy

02 Jun 2025 ### kubectl set context & use context Command Examples

30 May 2025

Read next

### Kubernetes Built-in AI/ML Features You Should Know

### Authenticate Azure CLI Using Service Principal (Tutorial)

### Kubernetes Init Containers: A Complete Guide

### How To Setup and Configures Proxy Server - Squid Proxy

### kubectl set context & use context Command Examples

Great! You’ve successfully signed up.

Welcome back! You've successfully signed in.

You've successfully subscribed to DevOpsCube – Easy DevOps, SRE Guides & Reviews.

Your link has expired.

Success! Check your email for magic link to sign-in.

Success! Your billing info has been updated.

Your billing was not updated.
