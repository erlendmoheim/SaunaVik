# Vikjasauna booking system

A booking application I developed for Vikjasauna, a sauna business in Vik, Norway. Customers and the owner used it until around summer 2026. The service has since been retired, and this repository preserves the implementation.

Development in this repository took place from December 2025 to February 2026.

## My contribution

I built the application's Java backend and web interface, including booking validation, availability calculations, administrator controls and Telegram notifications. I adjusted the application to the business's booking rules, including 90-minute sessions, capacity limits and closures for individual sessions or entire days.

## Features

- Customers can choose a date, see availability and reserve places in a session.
- The booking service validates dates, session times, contact details and available capacity.
- The owner can view bookings, delete bookings, close sessions or whole days, and reopen them.
- New bookings can trigger Telegram notifications. Notification failures are handled separately so they do not prevent a booking from being saved.
- Booking times use the Europe/Oslo time zone.

## How it is structured

Spring MVC controllers handle the customer and administrator pages. `BookingService` contains the availability and booking rules, and Spring Data JPA repositories store bookings and closures in H2. JSP pages and JavaScript provide the web interface. `TelegramNotifier` handles optional booking notifications.

The application is packaged as a WAR file. The Docker build uses Maven and Java 21, then runs the application in Tomcat 10.1. It was deployed on Railway.

## Technology

- Java 21 and Spring Boot 3.3.6
- Spring MVC, Spring Data JPA and H2
- JSP/JSTL, JavaScript and CSS
- Maven, Docker and Tomcat
- Telegram Bot API

## Configuration and build

The configuration uses these environment variables:

- `ADMIN_KEY` for administrator access. Set an explicit value before running the application.
- `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` for optional notifications. Notifications are disabled when these are empty.

See [application.properties](src/main/resources/application.properties) for configuration, [pom.xml](pom.xml) for the Maven build, and [Dockerfile](Dockerfile) for the container build and runtime. Running the application requires an appropriately configured environment and database storage.

## Limitations

This is a small historical application. The repository does not include automated tests. Administrator access uses a shared key; authentication hardening and tests for concurrent bookings would be useful next steps before reusing it for a new deployment.
