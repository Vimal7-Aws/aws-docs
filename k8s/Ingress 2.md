Paste your rich text content here. You can paste directly from Word or other rich text sources.

## What is Ingress?

### Ingress API

[](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#ingress-v1beta1-networking-k8s-io)[Ingress](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#ingress-v1beta1-networking-k8s-io) is an API resource that provides a simple way of describing HTTP and HTTPS routes from outside the cluster to [](https://kubernetes.io/docs/concepts/services-networking/service/)[services](https://kubernetes.io/docs/concepts/services-networking/service/) within the cluster. The basic idea behind the `Ingress` is to provide a way of describing higher level traffic management constraints, specifically for HTTP. With `Ingress`, we can define rules for routing traffic without creating a bunch of Load Balancers or exposing each service on the node. It can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL/TLS, and offer name-based virtual hosting and content-based routing. The name Ingress may be a bit misleading, since, at first glance it makes it seem as if it were only for north-south traffic, but actually it can be used for east-west traffic as well. Lets see a basic example:

```plaintext
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: echo-service
  namespace: default
spec:
  rules:
    - host: echo.example.com
      http:
        paths:
          - backend:
              serviceName: echo
              servicePort: 80
            path: /
```

### Ingress controller

Ingress is one of the built-in APIs which doesn't have a built-in controller, and an ingress controller is needed to actually implement the `Ingress API`. Ingress is made up of an Ingress API object and an Ingress controller. As mentioned earlier, Kubernetes Ingress is an API object that describes the desired state for exposing services deployed to a Kubernetes cluster. So, to make it work an Ingress controller you will require the actual implementation of the Ingress API to read and process the Ingress resource's information. An ingress controller is usually an application that runs as a pod in a Kubernetes cluster and configures a load balancer according to Ingress Resources. The load balancer can be a software load balancer running in the cluster or a hardware or cloud load balancer running externally. Different load balancers require different ingress controllers. Since the Ingress API is actually _just_ metadata, the Ingress controller does the heavy lifting. Various ingress controllers [](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/#additional-controllers)[are available](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/#additional-controllers) and it is important to choose the right one carefully for each use case. It's also possible to have multiple ingress controllers in the same cluster and to set a desired ingress controller for each Ingres. Usually, we end up using a combination of these controllers for different scenarios in the same cluster. For example, we may have one for handling the external traffic coming into the cluster which includes bindings to SSL certificates, and have another internal one with no SSL binding that handles in-cluster traffic. ![ingress_controller](https://outshift-headless-cms-s3.s3.us-east-2.amazonaws.com/blog/k8s-ingress/ingress-1.png "ingress_controller")

### Ingress examples

The Ingress [](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status)[spec](https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#spec-and-status) contains all the information needed to configure a load balancer or a proxy server. Most importantly, it contains a list of rules matched against all incoming requests. Ingress resources only support rules for directing HTTP traffic. Besides the base functionality the resource provides, the various ingress controller implementations usually provide several advanced features through custom resource annotations.

#### Single service ingress

An Ingress with no rules sends all traffic to a single default backend. If none of the hosts or paths match the HTTP request in the Ingress objects, the traffic is routed to the default backend.

```plaintext
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: echo-service
  namespace: default
spec:
  backend:
    serviceName: echo
    servicePort: 80
```

#### Simple fanout

A fanout configuration routes traffic based on the HTTP URI of the request. It allows us to use a single load balancer and IP address to serve multiple backend services. ![ingress-fanout](https://outshift-headless-cms-s3.s3.us-east-2.amazonaws.com/blog/k8s-ingress/ingress-fanout-1.png "ingress-fanout")

```plaintext
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-service
  namespace: default
spec:
  rules:
    - host: services.example.com
      http:
        paths:
          - backend:
              serviceName: service1
              servicePort: 80
            path: /service1
          - backend:
              serviceName: service2
              servicePort: 80
            path: /service2
```

#### Hostname based routing

Hostname-based routing supports having one load balancer to handle traffic for different hostnames pointing to the same IP address. ![ingress-host-based](https://outshift-headless-cms-s3.s3.us-east-2.amazonaws.com/blog/k8s-ingress/ingress-host-based-1.png "ingress-host-based")

```plaintext
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: public-services
  namespace: default
spec:
  rules:
    - host: service1.example.com
      http:
        paths:
          - backend:
              serviceName: service1
              servicePort: 80
            path: /
    - host: service2.example.com
      http:
        paths:
          - backend:
              serviceName: service2
              servicePort: 80
            path: /service2
```

#### TLS

Ingress can also provide TLS support, but it is limited to port 443 only. If the TLS configuration section in an Ingress specifies different hosts, they are multiplexed on the same port according to the hostname that's been specified through the SNI TLS extension (if the Ingress controller supports SNI). The TLS secret must contain keys named `tls.crt` and `tls.key`, which contain the certificate and private key for TLS. ![ingress-tls](https://outshift-headless-cms-s3.s3.us-east-2.amazonaws.com/blog/k8s-ingress/ingress-tls-1.png "ingress-tls")

```plaintext
apiVersion: v1
kind: Secret
metadata:
  name: public-services-tls
  namespace: default
data:
  tls.crt: base64 encoded cert
  tls.key: base64 encoded key
type: kubernetes.io/tls
```

Referencing this secret in an Ingress tells the Ingress controller to secure the channel from the client to the load balancer using TLS. We need to make sure the TLS secret we have created came from a certificate that contains a Common Name (CN), also known as a Fully Qualified Domain Name (FQDN) for `services.example.com`. 

```plaintext
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: services-with-tls
  namespace: default
spec:
  tls:
    - hosts:
        - services.example.com
      secretName: public-services-tls
  rules:
    http:
      paths:
        - backend:
            serviceName: service1
            servicePort: 80
          path: /service1
        - backend:
            serviceName: service2
            servicePort: 80
          path: /service2
```

### Multiple ingress controllers

As was briefly mentioned previously, it is possible to run multiple ingress controllers within a cluster. Each Ingress should specify a class to indicate which ingress controller should be used, if more than one exists within the cluster. Before Kubernetes 1.18 an annotation (`kubernetes.io/ingress.class`) was used to specify the ingress class. In 1.18 a new `ingressClassName` field has been added to the Ingress spec that is used to reference the `IngressClass` resource used to implement the Ingress.

```plaintext
apiVersion: networking.k8s.io/v1beta1
kind: IngressClass
metadata:
  name: external-lb
spec:
  controller: example.com/ingress-controller
  parameters:
    apiGroup: k8s.example.com/v1alpha
    kind: IngressParameters
    name: external-lb
```

IngressClass resources contain an optional parameters field. This can be used to reference additional configurations for the class in question. We can mark a particular IngressClass as the default for the cluster. Setting the `ingressclass.kubernetes.io/is-default-class` annotation to `true` on an IngressClass resource will ensure that new Ingresses, without an `ingressClassName` field specified, will be assigned to the default IngressClass.
