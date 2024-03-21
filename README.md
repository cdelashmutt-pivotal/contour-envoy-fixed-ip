# Fixed Envoy IP for Contour with NSX-T L4 LB

A small repo to showcase how to overlay a fixed IP for Envoy deployed with [Project Contour](https://projectcontour.io).

NSX-T's NCP controller uses the Kubernetes Service object's `spec.loadBalancerIP` field to allow you to specify a preferred "external" (to Kubernetes) IP to use for the service.  We can leverage Project Carvel's [ytt](https://carvel.dev/ytt/) project to add this field to the Envoy Service object definition, but accept any additions or changes to the manifest that the Project Contour committers make.

To use this overlay, clone this repo, or simply download the `overlay.yaml` file to your own working directory.  Then, you can issue the following command to use the latest Contour deployment yaml and overlay the `spec.loadBalancerIP` field with the value to a value you specify on the command line.  You can review the output of the overlay first by running (setting the IP to whatever you need):
```
ytt -f https://projectcontour.io/quickstart/contour.yaml -f overlay.yaml -v loadBalancerIP=10.11.12.13
```

If you are happy with the results, you can apply them with the following command:
```
ytt -f https://projectcontour.io/quickstart/contour.yaml -f overlay.yaml -v loadBalancerIP=10.11.12.13 | kubectl apply -f -
```