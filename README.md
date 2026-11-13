# Vikjasauna booking system

I built and deployed this booking system for a sauna business in Vik, Norway. Customers used it to reserve places, and the owner used it to manage bookings and close sessions.

## Booking flow

Customers choose a date and session and see how many places are available. On submission, [BookingService](src/main/java/no/vik/sauna/booking/BookingService.java) checks the date, contact details, capacity and closures before saving the reservation. The owner can manage bookings and close individual sessions or entire days. [TelegramNotifier](src/main/java/no/vik/sauna/common/TelegramNotifier.java) handles optional notifications separately so a failed notification does not prevent the booking from being saved.

I built the backend and web interface around the business's fixed session times and capacity rules. Spring MVC controllers serve the JSP pages. Spring Data JPA stores reservations and closures in H2. The application uses Java 21 and Spring Boot, and I deployed it on Railway with Docker and Tomcat.

## Run locally

The [Dockerfile](Dockerfile) builds a WAR with Maven and runs it in Tomcat on port 8080.

```sh
docker build -t saunavik .
export ADMIN_KEY="$(openssl rand -hex 24)"
docker run --rm -p 127.0.0.1:8080:8080 -e ADMIN_KEY saunavik
```

The command generates a key for local administrator access. To enable booking notifications, set `TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID` in your environment and pass them to Docker with `-e`. Open [localhost:8080](http://localhost:8080/) to try the booking flow. H2 stores its file in `./data` relative to the process. Use persistent storage if you want reservations to survive container replacement. See [application.properties](src/main/resources/application.properties) for configuration.

## Limitations

This repository contains the historical booking system. Administration uses a shared key, and the repository has no automated test suite. If I reused it for a new deployment, I would replace shared-key access and add tests for concurrent bookings.
