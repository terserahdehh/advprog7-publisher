# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. The Observer pattern in Head First Design Patterns uses an interface to allow different behaviors. In the BambangShop code, the Subscriber is just a struct, which is fine if all subscribers are the same. If different subscriber behaviors are needed later, using a trait would be a better choice.

2. Using a Vec means checking each element to ensure uniqueness, which is slow. A DashMap uses keys, so it can quickly enforce unique ids and urls. Therefore, using DashMap is the better choice here.

3. Using DashMap is still needed. The Singleton pattern handles instance creation but doesn't solve thread safety by itself. DashMap provides built in, efficient thread-safe access, which makes it more suitable for our use case.

#### Reflection Publisher-2

1. Separating Service and Repository from the Model keeps each part focused on one job. The Model defines the data structure, the Repository handles data storage and retrieval, and the Service manages the business rules and logic. This separation makes the code easier to maintain, test, and update.

2. If we only use the Model, every function would mix together. For example, the Program, Subscriber, and Notification models would handle not only their data but also saving, updating, and business logic. This mix makes the code more complex because each model would need to know too much about the system. It would be harder to change one part without breaking another, testing would become difficult, and the overall design would be less clear.

3. Yes, I've used Postman. It lets me quickly send requests and view responses for our API endpoints. 

#### Reflection Publisher-3
1. This implementation uses the push model. The publisher sends notifications directly to subscribers when an event happens. Subscribers receive the data without having to request it.

2. #### Advantages of Pull Model:

- Subscribers can fetch only the data they need, reducing unnecessary data transfer.

- They have control over the timing of updates, which can optimize resource usage.

- It can lead to less noise if subscribers are interested in only specific changes.

    #### Disadvantages of Pull Model:

- It adds complexity to the system as subscribers must manage when and how to request data.

- There may be increased latency because updates aren't received immediately.

- Frequent pull requests by many subscribers could lead to higher server load and potential synchronization issues.

3. Without multi-threading, the notification process would run sequentially. This means each subscriber update would block the main thread until it finishes. The overall performance and responsiveness of the program could decrease, especially with many subscribers.
