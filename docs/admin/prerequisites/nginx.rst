.. _nginx:

NGINX ingress
============================

Renku provides some `kubernetes services <https://kubernetes.io/docs/concepts/services-networking/service/>`_ that need to be exposed. Traffic in Kubernetes can be managed and controlled using different tools.
The setup of Renku assumes that `NGINX Ingress <https://www.nginx.com/products/nginx/kubernetes-ingress-controller/>`_ is deployed and running in the cluster.

Install the `nginx-ingress`:

.. code-block:: bash

   $ helm upgrade --install nginx-ingress stable/nginx-ingress --namespace kube-system -f helm-installs/nginx-values.yaml
   $ helm upgrade --install nginx-ingress stable/nginx-ingress --namespace kube-system --set controller.hostNetwork=true

Verify that the deployment works as expected:

.. code-block:: bash

  $ helm status nginx-ingress | grep -i port

The output of the previous command should be:

.. code-block:: bash

    export HTTP_NODE_PORT=32080
    export HTTPS_NODE_PORT=32443
    export NODE_IP=$(kubectl --namespace kube-system get nodes -o jsonpath="{.items[0].status.addresses[1].address}")
    echo "Visit http://$NODE_IP:$HTTP_NODE_PORT to access your application via HTTP."
    echo "Visit https://$NODE_IP:$HTTPS_NODE_PORT to access your application via HTTPS."
                  servicePort: 80

Now let's check that we can contact the ingress:

.. code-block:: bash

  $ curl -v http://<floating-ip>/
  default backend - 404
