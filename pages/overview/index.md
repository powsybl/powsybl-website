---
layout: default
---

# Overview
PowSyBl (<b>Pow</b>er <b>Sy</b>stem <b>Bl</b>ocks) is an open source framework written in Java,
dedicated to electrical grid modelling and simulation. 
It is part of the [LF Energy Foundation](https://www.lfenergy.org/), a project of The Linux Foundation that supports 
open source innovation projects within the energy and electricity sectors. 
PowSyBl in an open source framework licensed under the [Mozilla Public License 2.0](./license).

The power system blocks may be used through scripts for a quick shot, but also be assembled to build state of the art applications.
Indeed, one major aim of the project is to make it easy to write complex software for power 
system simulation and analysis. For example, using PowSyBl one can create applications able to:
- handle a variety of formats, such as CGMES for European data exchanges
- perform power flow simulations and security analyses on the network
- perform dynamic simulations on the network
- etc.

Another key characteristic of PowSyBl is its modular design, at the core of the open source approach.
It enables developers to extend or customize its features by providing their own plugins.

Check the [Getting started](../documentation/user) and [Configuration](../documentation/user/configuration) pages to learn how to install and configure PowSyBl.

# Features
PowSyBl provides a complete internal grid model with substations, voltage levels, AC and DC lines, two and three windings transformers, batteries,
generators, loads, shunt and static VAR compensators, etc. 

![Node breaker topology](./img/nodeBreakerTopology.svg){: width="50%" .center-image}

For security analyses, it offers the possibility to define contingencies on the network.
The grid model can also be enhanced with extensions that complete the equipments modeling 
(dynamic profile, short-circuit profile, monitoring, etc.). 

PowSyBl also provides importers and exporters for several common pan-european exchange 
formats (Entso-E CIM/CGMES, UCTE-DEF, etc.).


PowSyBl as a library provides several APIs for power systems’ simulation and analysis 
(power flow computation, security analysis, remedial action simulation, 
short circuit computation, sensitivity analysis, time domain simulation, etc.). 
These simulations can run either on a personal computer or on a server, but they can 
also run on a supercomputer like in the iTesla project with the Curie supercomputer. 
The separation of the simulation API and the implementations allows developers to 
provide their own implementations if necessary, which makes the framework very flexible.

PowSyBl is also available as a command line tool, for quick shot modelling and/or simulation.
PowSyBl scripts may be written and executed thanks to the dedicated Domain Specific Language
code included in the project.

All the features of PowSyBl are exposed as web services, so as to make it easy to build web-based 
applications on top of the framework. Mored details about the services are given below in the [Microservices](#microservices) section.

# Projects

![GitHub logo](./img/github-logo.png){: width="30%" .center-image}

## Java libraries
The PowSyBl project contains of a set of Java libraries that cover all the abovementioned features:

- [PowSyBl core](https://github.com/powsybl/powsybl-core): contains all the grid modelling features, 
covering all the publicly supported formats

- [Open loadflow](https://github.com/powsybl/powsybl-open-loadflow): an open source library for power
flow simulation

- [PowSyBl Dynawo](https://github.com/powsybl/powsybl-dynawo): an API to run time domain simulations
with the open source [Dynawo](https://github.com/dynawo/) software

- [AFS](https://github.com/powsybl/powsybl-afs): a library to handle study files

- [PowSyBl HPC](https://github.com/powsybl/powsybl-hpc): a library to facilitate high performance computing
with PowSyBl

- [Balances adjustments](https://github.com/powsybl/powsybl-balances-adjustment): 
balances adjustment is a process that consists in acting on 
specified injections to ensure given balance on specific network areas.

- [PowSyBl IIDM for C++](https://github.com/powsybl/powsybl-iidm4cpp): 
a C++ implementation of IIDM, to enable C++ developers to use PowSyBl. 
[PowSyBl math native](https://github.com/powsybl/powsybl-math-native) may be used 
together with it for C++ matrix inversion.

- [PowSyBl GSE](https://github.com/powsybl/powsybl-gse) ([archived]()): a library to make it easier to 
write desktop applications based on PowSyBl. This repository is archived. The Grid Study Environment (GSE) project
aimed at helping developers to write desktop applications based on PowSyBl, through a JavaFX user interface.

All these repositories also contain the associated DSL when necessary, for a very simple use of PowSyBl through the command line.

## Microservices
The project also contains a set of microservices that expose PowSyBl's features:

- [Case server](https://github.com/powsybl/powsybl-case): handles raw network data storage.
It relies on a file system to store the data and makes it possible to upload 
and fetch networks in any format supported by PowSyBl (CGMES, UCTE, XIIDM, etc).

- [Network store server](https://github.com/powsybl/powsybl-network-store): provides a persistent IIDM implementation.
Almost all the features developed in PowSyBl rely on the IIDM API for accessing network data. 
The idea of this service is to re-implement the IIDM API for a better integration in a 
typical microservice architecture. Instead of an in-memory implementation like in the 
PowSyBl core repository, this implementation is backed by a Cassandra database. 
The network model is stored in the database in a structured way 
(one table per equipment type and with indexes), so that we can query only the needed 
data and ensure good performance for common operations. Examples of common operations 
are a simple switch position change (only a few values), 
a single line diagram rendering (substation wide data) or a loadflow run (network wide data). 
The REST interface exposed by this service is very low-level for performance reasons. 
In particular, the write operations do not prevent inconsistent modifications. 
Therefore, network data should usually be accessed from this service using the provided 
Java client (the one which implements the IIDM API) and not directly from the REST 
interface because the Java client is safe and prevents inconsistent modifications.

- [Geographical data server](https://github.com/powsybl/powsybl-geo-data): handles equipments locations.
Substations and line paths positions can be stored and queried using this geographical data server. 
A Cassandra database is used to store the data. 
Data can be indexed by countries and nominal voltages. 
This service is typically used to display a network on a map. 
This service abstracts away the different sources of geographical data.

- [Network conversion server](https://github.com/powsybl/powsybl-network-conversion-server): 
this service can convert a network stored in a case server to an IIDM network 
stored in the network store server.

- [CGMES geographical data import server](https://github.com/powsybl/powsybl-cgmes-gl): CGMES geographical data import server.
The CGMES-GL profile contains substations and lines geographical positions. 
This service can take a CGMES network containing a Geographical Layout profile from a case server 
and upload its content to the geographical data server.

- [Single line diagram server](https://github.com/powsybl/powsybl-single-line-diagram-server): 
this service can generate a voltage level single line diagram (in SVG format) for a 
given voltage level in an IIDM network, from a network store server.

- [Network map server](https://github.com/powsybl/powsybl-network-map-server):
this service is used to extract network data from a network store server 
and reshape it to feed a UI network map component.

<!--
Loadflow Server

The load flow server is able to run a load flow on a network from 
a network store server and update the state variables.

Network modification server

This is a high level network modification service. It can apply a list of predefined network modifications (switch position, setpoint, tap position, etc) or execute a Groovy script when a more generic and powerful way to modify the network is needed.

### Study server

This is the unique entry point for the front end. This service is responsible for study management (creation, opening, removal) and also exposes all operations from other services needed for the front end.

### Study front-end

Study tool front end developped in React.js.

</li-->

## Tutorials
PowSyBl provides a repository containing all the necessary code to go through its tutorials: 
[PowSyBl tutorials](https://github.com/powsybl/powsybl-tutorials).
 
## Incubator

The [PowSyBl incubator](https://github.com/powsybl/powsybl-incubator) repository contains
all the experimental code corresponding to ongoing work that is not stabilized yet.
