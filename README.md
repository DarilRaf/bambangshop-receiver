# BambangShop Receiver App

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
4.  `repository`: this module contains structs that serve as databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a Rocket web framework skeleton that you can work with.

As this is an Observer Design Pattern tutorial repository, you need to implement a feature: `Notification`.
This feature will receive notifications of creation, promotion, and deletion of a product, when this receiver instance is subscribed to a certain product type.
The notification will be sent using HTTP POST request, so you need to make the receiver endpoint in this project.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Receiver" folder.

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment

1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    ROCKET_PORT=8001
    APP_INSTANCE_ROOT_URL=http://localhost:${ROCKET_PORT}
    APP_PUBLISHER_ROOT_URL=http://localhost:8000
    APP_INSTANCE_NAME=Safira Sudrajat
    ```
    Here are the details of each environment variable:
    | variable | type | description |
    |-------------------------|--------|-----------------------------------------------------------------|
    | ROCKET_PORT | string | Port number that will be listened by this receiver instance. |
    | APP_INSTANCE_ROOT_URL | string | URL address where this receiver instance can be accessed. |
    | APP_PUUBLISHER_ROOT_URL | string | URL address where the publisher instance can be accessed. |
    | APP_INSTANCE_NAME | string | Name of this receiver instance, will be shown on notifications. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)
3.  To simulate multiple instances of BambangShop Receiver (as the tutorial mandates you to do so),
    you can open new terminal, then edit `ROCKET_PORT` in `.env` file, then execute another `cargo run`.

    For example, if you want to run 3 (three) instances of BambangShop Receiver at port `8001`, `8002`, and `8003`, you can do these steps:

    - Edit `ROCKET_PORT` in `.env` to `8001`, then execute `cargo run`.
    - Open new terminal, edit `ROCKET_PORT` in `.env` to `8002`, then execute `cargo run`.
    - Open another new terminal, edit `ROCKET_PORT` in `.env` to `8003`, then execute `cargo run`.

## Mandatory Checklists (Subscriber)

- [x] Clone https://gitlab.com/ichlaffterlalu/bambangshop-receiver to a new repository.
- **STAGE 1: Implement models and repositories**
  - [x] Commit: `Create Notification model struct.`
  - [x] Commit: `Create SubscriberRequest model struct.`
  - [x] Commit: `Create Notification database and Notification repository struct skeleton.`
  - [x] Commit: `Implement add function in Notification repository.`
  - [x] Commit: `Implement list_all_as_string function in Notification repository.`
  - [x] Write answers of your learning module's "Reflection Subscriber-1" questions in this README.
- **STAGE 3: Implement services and controllers**
  - [x] Commit: `Create Notification service struct skeleton.`
  - [x] Commit: `Implement subscribe function in Notification service.`
  - [x] Commit: `Implement subscribe function in Notification controller.`
  - [x] Commit: `Implement unsubscribe function in Notification service.`
  - [x] Commit: `Implement unsubscribe function in Notification controller.`
  - [x] Commit: `Implement receive_notification function in Notification service.`
  - [x] Commit: `Implement receive function in Notification controller.`
  - [ ] Commit: `Implement list_messages function in Notification service.`
  - [ ] Commit: `Implement list function in Notification controller.`
  - [ ] Write answers of your learning module's "Reflection Subscriber-2" questions in this README.

## Your Reflections

This is the place for you to write reflections:

### Mandatory (Subscriber) Reflections

#### Reflection Subscriber-1

1.Using RwLock<> (Read-Write Lock) is necessary in this case because we need to allow multiple threads to read the Vec of Notifications concurrently, while still protecting the vector from concurrent modifications. The RwLock<> provides a way to achieve this by allowing multiple reader threads to access the data simultaneously, but only one writer thread can modify the data at a time.

On the other hand, Mutex<> (Mutual Exclusion) is a simpler synchronization primitive that allows only one thread to access the protected data at a time, regardless of whether the access is for reading or writing. If we were to use Mutex<> in this case, it would unnecessarily serialize all access to the Vec of Notifications, even for read operations, which could lead to performance issues, especially if there are many concurrent read requests.

By using RwLock<>, we can take advantage of the fact that multiple threads can safely read the Vec of Notifications simultaneously, improving the overall performance and concurrency of the application.

2.In Rust, static variables are immutable by default, which means their values cannot be changed after they are initialized. This design decision is made to ensure thread-safety and prevent data races, which can lead to undefined behavior and potential crashes.

Unlike Java, where you can mutate the content of a static variable through a static function or block, Rust does not allow this kind of mutation for static variables. Instead, Rust provides safer alternatives, such as using thread-safe data structures like Mutex, RwLock, or atomic types, to manage shared mutable state across threads.

In the case of this tutorial, we used the lazy_static crate to define the NOTIFICATIONS vector and SUBSCRIBERS map as "lazy static" variables. These variables are initialized only once, during the first time they are accessed, and their initialization is guaranteed to be thread-safe. However, to modify the contents of these data structures, we need to use the appropriate synchronization primitives, such as RwLock for the NOTIFICATIONS vector.

By enforcing immutability for static variables, Rust ensures that shared data cannot be accidentally modified in an unsafe manner, which could lead to data races and undefined behavior. This design choice promotes thread-safety and encourages the use of safe concurrency patterns from the outset.

#### Reflection Subscriber-2
