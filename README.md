
# Restful API in Symfony 4



## Objetives?

- *Changing Restful API in Symfony 4 to Layered Architecture*

## How to install?
- *composer install*
- *php bin/console doctrine:database:create*
- *php bin/console assets:install*

### Description about objetives

The code is organized by functional components, which have a logically coherent and tight coupling of the contained models. A component (e.g., a User component) is then organized into five different layers:
 
`Domain layer:` contains the actual business logic and domain models.

`Infrastructure layer:` binds the business logic implementation to infrastructure and frameworks, such as Symfony or Doctrine.

`Presentation layer:` is responsible for presenting a user interface, allowing users to make use of the business logic.

`Api layer:` provides an API interface to interact with the domain services.

`Tests layer:` Contains all tests relevant to the component.

## Domain

```
├── User
│   ├── Domain
│   │   ├── Command
│   │   ├── Event
│   │   ├── Model
│   │   ├── Repository
│   │   ├── Service
│   │   └── ValueObject
```
Most importantly, the domain layer contains the domain models, domain repositories (simple interactions with models), and domain services (complex interactions with [multiple] entities). Also command classes describing business interactions and corresponding domain events belong here. Last but not least, also custom value objects that are important to the domain are stored here.

All classes and interfaces defined in this layer have no dependencies to any third party library.

## Infrastructure

```
├── User
│   ├── Infrastructure
│   │   ├── UserInfrastructureBundle.php
│   │   ├── Console
│   │   ├── EventListener
│   │   ├── Repository
│   │   ├── Resources
│   │   └── Service
```
The infrastructure layer binds the elements defined in the domain layer to a specific framework or platform in order to have a runnable application. The layer can for example act as an adapter/wrapper for specific persistence tasks or provide application services (such as email, caching, message queues, etc.).

In case of Symfony, the infrastructure layer also contains console commands the application may have or EventListeners that listen to Symfony-specific or custom events through the frameworks event system.

In general, the layer contains the actual implementations of the business services described in the domain layer. Hence, also most dependency injection definitions point to implementations defined in this layer.

## API

```
├── User
│   ├── Api
│   │   ├── UserApiBundle.php
│   │   ├── Controller
│   │   └── Resources
```
The API layer provides an API interface to interact with the application. For example, this could be a REST API backend for a modern JavaScript-based frontend. The layer defines routes and controllers that trigger the corresponding operations in the domain layer.

Having the offered API separate from the implementation allows to precisely define which services are exposed to whom through the API. The layer handles authentication and authorization with the API and can also rate-limit requests or track metrics for billing purposes. Putting all these API concerns into this separate layer allows the domain and infrastructure layers to remain slim and focused, while the exposed API can easily be managed.


## Presentation

```
├── User
│   ├── Presentation
│   │   ├── UserPresentationBundle.php
│   │   ├── Controller
│   │   ├── Form
│   │   ├── Resources
│   │   └── Twig
```
Symfony provides a lot of resources to help implementing an HTML-based user interface. The presentation layer contains all resources concerned with creating a user interface rendered on the server side for the end user. JavaScript based user interfaces typically only require the API layer and there is thus no need for an extra presentation layer. For Symfony, this means that web based controllers, forms with input validation and Twig view scripts are stored in this layer.

Having separated the user interface from the domain and the infrastructure simplifies the development of either layer. The code in the presentation layer should not contain any business logic, but only forward calls to the respective services of the domain layer. Hence, the presentation layer stays small helping developers to easily evolve the user interface, even during big changes.


## Tests

```
├── User
│   └── Tests
│       ├── Api
│       ├── Domain
│       ├── Functional
│       ├── Infrastructure
│       └── Presentation
├── ...
```
The tests layer contains all tests relevant to the component. Tests are again, organized by layer. In addition to unit testing layers, the tests folder could also contain E2E (i.e. in the Functional folder) and integration tests. Putting all kinds of tests into this folder structure helps new developers find the tests they are looking for and to easily find components that might be lacking tests of a certain kind.

[Sources: https://www.fabian-keller.de/blog/domain-driven-design-with-symfony-a-folder-structure/]