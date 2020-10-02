# OpenLDAP and phpLDAPadmin using Kubernetes and Longhorn

Run a lightweight directory access protocol using openLDAP with Kubernetes.<br/>
Manage your PVC(s) with Longhorn.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

A K8s cluster with longhorn ready.

### Installing

Create both env and secret files for your admin password.

```bash
# Create the env file
cat <<< 'LDAP_ORGANISATION="EXAMPLE"
LDAP_DOMAIN=example.com' > env
kubectl create configmap openldap --from-env-file=env

# Create the secret file
cat <<< 'LDAP_ADMIN_PASSWORD=DFSe34csPs54519x' > secret
kubectl create secret generic openldap --from-env-file=./secret
```

Then create the objects (pvc, deployment, service).

```bash
kubectl apply -f pvc.yml 
# ...
```

These objects are voluntarily separated into several files.<br/>
The service is a NodePort by the way.

### Try it

You can create some ldif files and add them

```bash
# Suppose you have a users_ou.ldif in a ldifs folder
ldapadd -h Your_node_IP -p 31389 -D "cn=admin,dc=example,dc=com" \
-f ./ldifs/users_ou.ldif -W

# List everything
ldapsearch -h Your_node_IP -p 31389 -x -LLL -b dc=example,dc=com "*" \
-D "cn=admin,dc=example,dc=com" -W
```

## Built With

* [Kubernetes](https://kubernetes.io/) - Production-Grade Container Orchestration
* [Longhorn](https://longhorn.io) - Cloud native distributed block storage for Kubernetes
* Openldap & phpldapadmin - [Docker & Osixia] (https://github.com/osixia)

## Authors

* **Jeremie Payet** - *Initial work* - [JPCWeb](https://github.com/jpcweb)

## License

This project is licensed under the MIT License

## Acknowledgments

* [osixia](https://github.com/osixia)

