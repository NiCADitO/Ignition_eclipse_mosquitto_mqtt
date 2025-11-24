## ignition_eclipse_mosquitto_mqtt

This repository provides a self-contained, Dockerized environment for demonstrating an **MQTT architecture** using **Inductive Automation's Ignition** (Main and Edge Gateways) and the **Eclipse Mosquitto MQTT Broker**.

This setup is ideal for quick testing, development, and demonstration of **Sparkplug B** integration and general MQTT data transport between an Edge device (Ignition Edge) and a central server (Ignition Gateway) using a third-party broker (Mosquitto).

-----

## Repository Contents

| File Name | Description |
| :--- | :--- |
| `docker-compose.yaml` | Defines the multi-container application, including the two Ignition instances and the Mosquitto broker. |
| `mosquitto.conf` | Configuration file for the Mosquitto broker, enabling anonymous connections for this demonstration. |
| `README.md` | This file. |

-----

## 🛠️ Prerequisites

  * **Docker**
  * **Docker Compose**

-----

## ⚙️ Configuration Details

The `docker-compose.yaml` file sets up three services:

### 1\. `mosquitto` (MQTT Broker)

  * **Image:** `eclipse-mosquitto`
  * **Port:** Exposed on host port **1883** (`1883:1883`).
  * **Configuration:** Uses the provided `mosquitto.conf`, which sets:
    ```
    listener 1883
    allow_anonymous true
    ```
    This allows any client to connect to the broker on the default MQTT port without credentials.

### 2\. `main` (Ignition Standard Gateway)

  * **Image:** `inductiveautomation/ignition:8.1.45`
  * **Port:** Exposed on host port **9088** (`9088:8088`).
  * **Gateway Name:** `Main`
  * **Edition:** `standard`
  * **Admin Credentials:** **Username** `ad`, **Password** `p` (for development only).

### 3\. `edge` (Ignition Edge Gateway)

  * **Image:** `inductiveautomation/ignition:8.1.45`
  * **Port:** Exposed on host port **9099** (`9099:8088`).
  * **Gateway Name:** `Edge`
  * **Edition:** `edge`
  * **Admin Credentials:** **Username** `ad`, **Password** `p` (for development only).

-----

## Getting Started

Follow these steps to quickly launch and test the environment:

### 1\. Launch the Services

From the root directory of the repository, run the following command:

```bash
docker compose up -d
```

This will pull the necessary images and start all three containers (`main`, `edge`, and `mosquitto`) in detached mode.

### 2\. Access the Gateways

Wait a few minutes for the Ignition Gateways to fully initialize. You can then access them via your web browser:

  * **Ignition Main Gateway:** [http://localhost:9088](https://www.google.com/search?q=http://localhost:9088)
  * **Ignition Edge Gateway:** [http://localhost:9099](https://www.google.com/search?q=http://localhost:9099)

### 3\. Configure MQTT Connectivity

To demonstrate the architecture, you will need to configure the **MQTT Transmission** and **MQTT Engine** modules within the Gateways.

  * **On the `edge` Gateway (https://www.google.com/search?q=http://localhost:9099):**

      * Install the **MQTT Transmission** module.
      * Configure the MQTT Transmitter to connect to the broker using the hostname **`mosquitto`** (as defined in `docker-compose.yaml`). The port is `1883`.

  * **On the `main` Gateway (https://www.google.com/search?q=http://localhost:9088):**

      * Install the **MQTT Engine** module.
      * Configure the MQTT Engine to connect to the broker using the hostname **`mosquitto`**. The port is `1883`.

Once configured, the Edge Gateway will publish Sparkplug B data to the Mosquitto broker, and the Main Gateway will subscribe to and process that data, making it available in its Tag Provider.

-----

## Stopping and Cleanup

To stop and remove the containers and networks:

```bash
docker compose down
```

To remove the persistent data volumes as well (which resets the Ignition Gateways to their initial state):

```bash
docker compose down -v
```

Would you like to know more about configuring the MQTT modules in Ignition?
