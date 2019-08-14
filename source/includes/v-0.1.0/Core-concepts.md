
# Core-concepts

## Member

member is a microservice instances.

## Distributed environment

Distributed environment is collection of members that share services between them.

Each member have access to all the services that are shared in the distributed environment.

## Registry

Registry store all [endpoints](#endpoint) received from the discovery.

## LocalCall

When a microservice use it own services.

## RemoteCall

When a microservice use another microservice's services.

## SeedAddress

seedAddress is an [address](#address) of another microservice(seed).

## Seed
the seed is a microservice container that used as an entrypoint to the [distributed environment](#distributed-environment).
each microservice can act as a seed for other microservice containers.

