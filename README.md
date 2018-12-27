# Cool Store: Cloud-Native Microservices Application on Service Mesh

![Travis (.org)](https://travis-ci.org/vietnam-devs/coolstore-microservices.svg?branch=master)
[![Build Status](https://dev.azure.com/vietnam-devs/coolstore-microservices/_apis/build/status/vietnam-devs.coolstore-microservices)](https://dev.azure.com/vietnam-devs/coolstore-microservices/_build/latest?definitionId=1)
[![Price](https://img.shields.io/badge/price-FREE-0098f7.svg)](https://github.com/vietnam-devs/coolstore-microservices/blob/master/LICENSE)
[![OpenTracing Badge](https://img.shields.io/badge/OpenTracing-enabled-blue.svg)](http://opentracing.io)

> This project is inspired from [CoolStore project](https://github.com/jbossdemocentral/coolstore-microservice) by [JBoss Demo Central](https://github.com/jbossdemocentral) & [Red Hat Demo Central](https://gitlab.com/redhatdemocentral)

> The structure of README is inspired from [GoogleCloudPlatform Demo](https://github.com/GoogleCloudPlatform/microservices-demo).

CoolStore is a containerised polyglot microservices application consisting of services based on .NET Core, NodeJS and more running on Service Mesh.

It demonstrates how to wire up small microservices into a larger application using microservice architectural principals.

<details>
  <summary><strong>Presentation</strong></summary>

Our team uses this demo application to demonstrate Kubernetes, AKS, Istio and similar cloud-native technologies in events as following

- [From Microservices to Service Mesh - DevCafe event in July 2018](https://www.slideshare.net/ThangChung/from-microservices-to-service-mesh-devcafe-event-july-2018)
- [Service Mesh for Microservices- Vietnam Mobile Day event in June 2018](https://www.slideshare.net/ThangChung/service-mesh-for-microservices-vietnam-mobile-day-june-2017)
- [Avoid SPOF in Cloud-native Apps - Vietnam Web Summit event in December 2018](https://www.slideshare.net/ThangChung/avoid-single-point-of-failure-in-cloud-native-application)

</details>

# Table of contents

* [Prerequisites](https://github.com/vietnam-devs/coolstore-microservices#prerequisites)
* [Overall Architecture of µServices](https://github.com/vietnam-devs/coolstore-microservices#overall-architecture-of-µservices)
* [Installation](https://github.com/vietnam-devs/coolstore-microservices#installation)
* [µService Development](https://github.com/vietnam-devs/coolstore-microservices#µmicroservice-development)
* [Open API](https://github.com/vietnam-devs/coolstore-microservices#open-api)
* [CI/CD](https://github.com/vietnam-devs/coolstore-microservices#ci-cd)
* [Service Mesh](https://github.com/vietnam-devs/coolstore-microservices#service-mesh)
* [Contributing](https://github.com/vietnam-devs/coolstore-microservices#contributing)
* [Contributors](https://github.com/vietnam-devs/coolstore-microservices#contributors)
* [Licence](https://github.com/vietnam-devs/coolstore-microservices#licence)

## Prerequisites

- **Windows 10:** The OS for developing and building this demo application .
- **[Windows Subsystem Linux - Ubuntu OS](https://docs.microsoft.com/en-us/windows/wsl/install-win10)**: The subsystem that helps to run easily the bash shell on Windows OS.
- **[Docker for Desktop (Kubernetes enabled)](https://www.docker.com/products/docker-desktop):** The easiest tool to run Docker, Docker Swarm and Kubernetes on Mac and Windows.
- **[Kubernetes](https://kubernetes.io)/[AKS](https://docs.microsoft.com/en-us/azure/aks):**
  The app is designed to run on Kubernetes (both locally on "Docker for
  Desktop", as well as on the cloud with AKS).
- **[Istio](https://istio.io):** Application works on Istio service mesh.
- **[helm](https://helm.sh)**: The best package manager to find, share, and use software built for Kubernetes. 
- **[skaffold](https://github.com/GoogleContainerTools/skaffold)**: Easy and Repeatable Kubernetes Development.
- **[NetCoreKit](https://github.com/cloudnative-netcore/netcore-kit):** Set of Cloud-native tools and utilities for .NET Core.

### Overall Architecture of µServices

![Architecture Screenshot](assets/images/arch-diagram.png?raw=true 'Architecture Diagram')

There are several individual µservices and infrastructure components that make up this app:

<table>
  <thead>
    <th>No.</th>
    <th>Service</th>
    <th>Description</th>
    <th>Source</th>
    <th>Endpoints</th>
  </thead>
  <tbody>
    <tr>
      <td align="center">1.</td>
      <td>IdP (.NET Core + In-memory database)</td>
      <td>Uses <a href="https://github.com/IdentityServer/IdentityServer4">IdentityServer4</a> to authentication with OAuth 2.0 and OpenID Connect for the whole stack</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/idp">code</a>
      </td>
      <td>
        <a href="http://localhost:5001">dev</a> & <a href="http://id.coolstore.local">staging</a>
      </td>
     </tr>
    <tr>
      <td align="center">2.</td>
      <td>Catalog (Node.js + Mongo)</td>
      <td>Serves products and prices for retail products</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/catalog">code</a>
      </td>
      <td>
        <a href="http://localhost:5002">dev</a> & <a href="http://api.coolstore.local/catalog/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">3.</td>
      <td>Cart (.NET Core + MySQL)</td>
      <td>Manages shopping cart for each customer</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/cart">code</a>
      </td>
      <td>
        <a href="http://localhost:5003">dev</a> & <a href="http://api.coolstore.local/cart/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">4.</td>
      <td>Inventory (.NET Core + MySQL)</td>
      <td>Serves inventory and availability data for retail products</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/inventory">code</a>
      </td>
      <td>
        <a href="http://localhost:5004">dev</a> & <a href="http://api.coolstore.local/inventory/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">5.</td>
      <td>Pricing (.NET Core + MySQL)</td>
      <td>Handles a business rules application for product pricing</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/pricing">code</a>
      </td>
      <td>
        <a href="http://localhost:5005">dev</a> & <a href="http://api.coolstore.local/pricing/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">6.</td>
      <td>Review (.NET Core + MySQL)</td>
      <td>Runs for writing and displaying reviews for products</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/review">code</a>
      </td>
      <td>
        <a href="http://localhost:5006">dev</a> & <a href="http://api.coolstore.local/review/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">7.</td>
      <td>Rating (Node.js + Mongo)</td>
      <td>Runs for rating products</td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/services/rating">code</a>
      </td>
      <td>
        <a href="http://localhost:5007">dev</a> & <a href="http://api.coolstore.local/rating/swagger">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">8.</td>
      <td>Web UI (Obsoleted)</td>
      <td>Frontend based on <a href="https://blazor.net">Blazor</a> - Full-stack web development with C# and <a href="https://webassembly.org/">WebAssembly</a></td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/WebUI">code</a>
      </td>
      <td>
        <a href="http://localhost:8080">dev</a> & <a href="http://web.coolstore.local">staging</a>
      </td>
     </tr>
     <tr>
      <td align="center">9.</td>
      <td>Web UI (PWA - Vuejs + Node.js)</td>
      <td>Frontend based on <a href="https://vuejs.org">vuejs</a> and <a href="https://nodejs.org">Node.js</a></td>
      <td>
        <a href="https://github.com/vietnam-devs/coolstore-microservices/tree/master/src/web">code</a>
      </td>
      <td>
        <a href="http://localhost:8080">dev</a> & <a href="http://web.coolstore.local">staging</a>
      </td>
     </tr>
     
  </tbody>
</table>

## Installation

<details>
  <summary><strong>Development environment</strong>: Up and Running locally with "Docker for Desktop"</summary>

1. Make sure we have **`Docker for Desktop`** running with **`Kubernetes`** option enabled. We need to install **`kubectl`**, **`helm`** and **`istioctl`** on the build machine as well.

2. From current console, type `bash` to enter `Linux Subsystem (Ubuntu)`

3. Then `cd` into your root of project

```
> ./deploys/scripts/build-all-images.sh
```

It should run and package all docker images.

4. Download and install [istio-1.0.0](https://github.com/istio/istio/releases/tag/1.0.0) on the box, and unzip it into somewhere, then initialize it with following commands

```
> cd <istio-1.0.0 path>
> kubectl create -f install/kubernetes/helm/helm-service-account.yaml
> helm init --service-account tiller --upgrade
> helm install install/kubernetes/helm/istio --name istio --namespace istio-system
```

More information about installing `istio` can be found at https://istio.io/docs/setup/kubernetes/helm-install

5. Apply `istioctl` command to `coolstore` chart (please create k8s folder in folder deploys)

```
> helm template deploys/charts/coolstore -f deploys/charts/coolstore/values.dev.yaml > deploys/k8s/coolstore.local.yaml
> istioctl kube-inject -f deploys/k8s/coolstore.local.yaml | kubectl apply -f -
```

6. Add hosts file with following content

```
127.0.0.1 api.coolstore.local
127.0.0.1 id.coolstore.local
127.0.0.1 coolstore.local
```

Waiting for the container provision completed

7. Install `coolstore-istio` chart

```
> helm install deploys\charts\coolstore-istio --name coolstore-istio
```

8. Access to following URLs

```
> curl -I http://coolstore.local # website
> curl -I http://api.coolstore.local # api gateway
> curl -I http://id.coolstore.local # identity provider
```

9. Clean up `coolstore` chart as

```
> kubectl delete -f deploys/k8s/coolstore.local.yaml
> helm delete coolstore-istio --purge
> helm delete istio --purge
```

**Notes**:

1. Global path
> Set `PATH` for `docker`, `kubectl`, `helm`, and `istioctl`.

2. Run with Nginx (not recommendation)
> If you want to run just only `Kubernetes` + `nginx-ingress` go to `deploys/charts/coolstore/values.yaml`, and modify as following
>```
> nginx:
>    enabled: true
>```
> Then run the `helm` command as
> ```
> helm install --name cs-nginx stable/nginx-ingress
> ```

</details>

<details>
  <summary><strong>Staging and Production environments</strong>: Up and Running on Azure Kubernetes Service (AKS)</summary>

[5 steps to bring CoolStore’s Service Mesh to Azure Kubernetes Service](https://medium.com/@thangchung/5-steps-to-bring-coolstores-service-mesh-to-azure-kubernetes-service-aks-9cd1a5aa008a)

</details>

## µService Development

Guidance for developing µService can be found at [Clean Domain-Driven Design in 10 minutes](https://medium.com/@thangchung/clean-domain-driven-design-in-10-minutes-6037a59c8b7b)

![µService Screenshot](assets/images/miniservice-development.PNG?raw=true 'Microservice')

## Open API

![OpenAPI Screenshot](assets/images/open-api.png?raw=true 'OpenAPI')

## CI/CD

![Lift and Shift](assets/images/lift-and-shift.PNG?raw=true 'liftandshift')

## Service Mesh

[Istio](https://istio.io) provide a wealth of benefits for the organizations that use them. There’s no denying, however, that adopting the cloud can put strains on DevOps teams. Developers must use microservices to architect for portability, meanwhile operators are managing extremely large hybrid and multi-cloud deployments. Istio lets you connect, secure, control, and observe services.

### Distributed Tracing

![DAG chart](assets/images/jaeger-dag-1.PNG?raw=true 'DAG')

![Trace chart](assets/images/jaeger-trace-1.PNG?raw=true 'Trace')

### Metrics

![Metrics chart](assets/images/grafana-ui-1.PNG?raw=true 'Metrics')

## Screenshots

<details>
  <summary>Home page</summary>

![home-page](assets/images/ui-screen-1.PNG?raw=true)

</details>

<details>
  <summary>Cart page</summary>

![cart-page](assets/images/ui-screen-2.PNG?raw=true)

</details>

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request :p

## Contributors

<table>
  <tbody>
    <tr>
      <td align="center" valign="top">
        <img width="150" height="150" src="https://github.com/thangchung.png?s=150">
        <br>
        <a href="https://github.com/thangchung">Thang Chung</a>
        <p>Core Developer</p>
      </td>
      <td align="center" valign="top">
        <img width="150" height="150" src="https://github.com/thinhnotes.png?s=150">
        <br>
        <a href="https://github.com/thinhnotes">Thinh Nguyen</a>
        <p>Core Developer</p>
      </td>
      <td align="center" valign="top">
        <img width="150" height="150" src="https://github.com/stuartleeks.png?s=150">
        <br>
        <a href="https://github.com/stuartleeks">Stuart Leeks</a>
        <p>Contributor</p>
      </td>
      <td align="center" valign="top">
        <img width="150" height="150" src="https://github.com/trumhemcut.png?s=150">
        <br>
        <a href="https://github.com/trumhemcut">Phi Huynh</a>
        <p>Contributor</p>
      </td>
     </tr>
  </tbody>
</table>

## Licence

Code released under [the MIT license](https://github.com/vietnam-devs/coolstore-microservices/blob/master/LICENSE).
