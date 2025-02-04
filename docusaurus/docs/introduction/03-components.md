---
id: components
title: Components
---

## Deployed Components

### Ziti Controller

The Ziti Controller is the central function of the
Ziti Network. The Ziti Controller provides the
configuration plane. It is responsible for configuring Ziti services
as well as being the central point for managing the identities
used by users, devices and the nodes making up the Ziti Network.
Lastly but critically, the Ziti Controller is responsible for
authentication and authorization for every connection in the Ziti
network.

The Ziti Controller must be configured with public key infrastructure
(pki). The configured pki is used to create secure, mutually
authenticated TLS (mTLS) network connections between any two
pieces of the Ziti Network. The Ziti Controller does not provide its
own pki but for the Ziti Controller to sign certificate requests (CSR)
the Ziti Controller will need to be configured with a key and
certificate used for signing. (Optionally, the Ziti CLI can be used
to generate a pki if needed)

The Ziti Controller also supports using a third-party pki should the
operator of the Ziti Network have an existing pki they wish to
reuse. Utilizing a third-party CA pushes the burden of obtaining
and distributing properly signed certificates to the operator of
the Ziti network but for sophisticated customers this might make
overall management of the network easier.
The Ziti Controller uses an out of process database (Postgres) to
store the information needed to manage the network.

### Ziti Fabric Router

Ziti Fabric Routers are the fundamental building blocks of the Ziti
Network. These routers are responsible for securely and reliably
delivering traffic from one Ziti Network node to the traffic’s
destination.

Ziti Fabric Routers are linked together to form a mesh network. This mesh is
constantly being monitored for latency and the fastest paths are
used when routing traffic to the destination. The monitoring also
allows for active failover to ensure a reliable network connection
even in the case of a node failure.

### Ziti Edge Router

Another fundamental building block of the Ziti Network is the
Ziti Edge Router. The Ziti Edge Router is the entry point for Edge
Clients connecting to the Ziti Network. The Ziti Edge Router is a
specialized Ziti Router incorporating the functionality of a Ziti Router to
enable it to route traffic over the Ziti network as a Ziti Router would
to a given destination.

The Ziti Edge Router in combination with the Ziti Controller is responsible
for authenticating and authorizing Ziti Edge Clients.

### Ziti Edge Clients

Connecting to the Ziti Network requires a Ziti Edge Client. Edge
Clients are designed to work with both brownfield and greenfield
applications.

If the solution being developed includes developing new
software Ziti offers SDKs targeting various languages
and runtimes to provide fast, reliable and secure connectivity.
These SDKs provide the capabilities needed to securely connect
to the Ziti Network and are designed to be easily incorporated
into the target application.

When adding secure connectivity to an already existing solution
Ziti offers specialized Edge Clients called tunnelers
which provide seamless, secure connectivity and do not require
changes to the target application.

## Logical Components

Once the Ziti Network is established and deployed the next step
is to configure the software-powered network. The three main
concepts necessary to configure a Ziti Network are: Identities,
Services, and Policies.

### Services

A service encapsulates the definition of any resource that could
be accessed by a client on a traditional network. A Ziti Service is
defined by a strong, extensible identity, rather than by an
expression of an underlay concept. This means that services
defined on a Ziti Network have an almost limitless "namespace"
available for identifying services. A Ziti Service is defined by a
name and/or a certificate, rather than by a DNS name or an IP
address (underlay concepts). Services also declare a node where
traffic that exits the Ziti Network needs to be sent do before
exiting. It’s possible for the node traffic enters to be the same it
exits and it’s possible for traffic needing to traverse the Ziti
Network Routers to reach the correct node. Simply specifying the
node is all the end-user need do, the Ziti Network handles the
rest.

### Identities

Identities represent individual endpoints in the Ziti Network
which can establish connectivity. All connections made within the
Ziti Network are mutually authenticated using X509 Certificates.
Every Identity is mapped to a given certificate’s signature. Ziti
Edge Clients present this certificate when initiating connections
to the Ziti Network. The presented certificate is used by the Ziti
Network to authorize the client and enumerate the services the
Identity is authorized to use.

### Policies
Policies control how Identities, Services and Edge Routers are allowed
to interact. In order to use a service the identity must be granted
access to the service. Also, since all access to a service goes through
one more edge routers, both the service and the identity must be
granted to access to the same edge router or edge routers.  

#### Role Attributes
Entities such as identities, services and edge routers can be added to 
policies explicity, either by id or name. Entities can  also be tagged 
with role attributes. Role attributes are simple strings like `sales`,
`Boston`, `us-employees` or `support`. Their meaning is decided by the 
administrator. Policies can include entities by specifying a set of role 
attributes to match.

#### Service Policies
Service Policies encapsulate the mapping between identities and 
services in a software-powered network. In the simplest terms, 
Service Policies are a group of services and a group of identities. 
The act of adding a service to a Service Policy will grant the 
identities in that Service Policy access to the given service. 
Similarly, adding an identity to a Service Policy will grant that
identity access to the services mapped in that Service Policy.

Service policies controls both which identities may dial a service (use the service)
and which identities may bind a service (provide or host the service). 
Each Service Policy may either grant dial or bind access, but not both.  

#### Edge Router Policies
Edge Router Policies manage the mapping between identities and 
edge routers. Edge Router Policies are a group of edge routers 
and a group of identities. Adding an edge router to an Edge
Router Policy will grant the identities in that Edge Router 
Policy access to the given edge router. Similarly, adding an identity 
to an Edge Router Policy will grant that identity access to the 
edge routers mapped in that Edge Router Policy. 

#### Service Edge Router Policies
Service Edge Router Policies manage the mapping between services and 
edge routers. Service Edge Router Policies are a group of edge routers 
and a group of services. Adding an edge router to a Service Edge
Router Policy will grant the services in that Service Edge Router 
Policy access to the given edge router. Similarly, adding a service 
to a Service Edge Router Policy will grant that service access to the 
edge routers mapped in that Service Edge Router Policy.
