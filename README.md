## ignition_eclipse_mosquitto_mqtt

Dockerized environment for demoing **MQTT** using an Ignition Main and Edge Gateway and the Eclipse Mosquitto MQTT Broker.

This setup is for quick testing, development, and demonstration of Sparkplug B and general MQTT data between an Edge and a Central server using a third-party broker Mosquitto.

-----

## Repository Contents

| File Name | Description |
| :--- | :--- |
| `docker-compose.yaml` | Defines the multi-container application, including the two Ignition instances and the Mosquitto broker. |
| `mosquitto.conf` | Configuration file for the Mosquitto broker, enabling anonymous connections for this demonstration. |
| `README.md` | This file. |

-----

## ⚙️ Configuration Details

The `docker-compose.yaml` file sets up three containers:

### 1\. `mosquitto` (MQTT Broker)

  * **Image:** `eclipse-mosquitto`
  * **Port:** Exposed on host port **1883** (`1883:1883`).
  * **Configuration:** Uses the provided `mosquitto.conf`, which sets:
    ```
    listener 1883
    allow_anonymous true
    ```
    This allows any client to connect to the broker on the default MQTT non-TLS port.

### 2\. `main` (Standard Gateway)

  * **Image:** `inductiveautomation/ignition:8.1.45`
  * **Port:** Exposed on host port **9088** (`9088:8088`).
  * **Gateway Name:** `Main`
  * **Edition:** `standard`
  * **Admin Credentials:** **Username** `admin`, **Password** `password` (for development only).

### 3\. `edge` (Edge Gateway)

  * **Image:** `inductiveautomation/ignition:8.1.45`
  * **Port:** Exposed on host port **9099** (`9099:8088`).
  * **Gateway Name:** `Edge`
  * **Edition:** `edge`
  * **Admin Credentials:** **Username** `admin`, **Password** `password` (for development only).

-----

## Getting Started

Follow these steps to launch:

### 1\. Launch the containers

Run the following command from the same directory as the .yaml file:

```bash
docker compose up -d
```

This will pull the necessary images and start all three containers (`main`, `edge`, and `mosquitto`) in detached mode.

### 2\. Access the Gateways

Wait for the Ignition Gateways to fully initialize. You can then access them via the browser:

  * **Ignition Main Gateway:** [http://localhost:9088](https://www.google.com/search?q=http://localhost:9088)
  * **Ignition Edge Gateway:** [http://localhost:9099](https://www.google.com/search?q=http://localhost:9099)

### 3\. Configure MQTT Connectivity

You will need to configure the **MQTT Transmission** and **MQTT Engine** modules within the Gateways.

  * **On the `edge` Gateway (https://www.google.com/search?q=http://localhost:9099):**

      * Install the **MQTT Transmission** module.
      * Configure the MQTT Transmitter to connect to the broker using the hostname **`mosquitto`** (as defined in `docker-compose.yaml`). The port is `1883`.

  * **On the `main` Gateway (https://www.google.com/search?q=http://localhost:9088):**

      * Install the **MQTT Engine** module.
      * Configure the MQTT Engine to connect to the broker using the hostname **`mosquitto`**. The port is `1883`.

Once configured, the Edge Gateway will publish Sparkplug B data to the Mosquitto broker, and the Main Gateway will subscribe to and process that data, making it available in the Cirrus Link managed Tag Provider.

-----

## Stopping and Cleanup

To stop and remove the containers and networks:

```bash
docker compose down
```

To remove the persistent data volumes as well (which resets the Ignition Gateways to their default state):

```bash
docker compose down -v
```

### Additional Documentation

- https://docs.chariot.io/display/CLD80/MQTT+Engine
- https://docs.chariot.io/display/CLD80/MQTT+Transmission
- https://www.docs.inductiveautomation.com/docs/8.1/platform/docker-image
