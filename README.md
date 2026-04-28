# Smart Taxi Server

Smart Taxi Server is a backend API for a taxi booking and tracking system, built using Django and Tastypie. It provides endpoints for managing user accounts, taxi locations, client requests, and push notifications for mobile devices.

## Features

*   **Location Tracking:** Tracks real-time GPS coordinates of taxis.
*   **Proximity Search:** Find available taxis within a specific radius of a user using Haversine formula.
*   **Booking System:** Allows clients to request a taxi and receive notifications.
*   **Push Notifications:** Integrates with Google Cloud Messaging (GCM) for sending real-time updates and notifications to devices.
*   **API Keys:** Uses API Key authentication and authorization for securing endpoints.
*   **RESTful API:** Exposes RESTful endpoints via Django Tastypie.

## Technology Stack

*   **Django** (1.5.1) - The core web framework.
*   **Django Tastypie** (0.9.15) - To create the REST APIs.
*   **django-gcm** (0.9.3) - For Google Cloud Messaging push notifications.
*   **PostgreSQL / dj-database-url** - Database support.
*   **South** - Database migrations.
*   **Gunicorn / Gevent** - Application server.

## Installation

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd smart-taxi-server
    ```

2.  **Create a virtual environment and activate it:**
    ```bash
    virtualenv venv
    source venv/bin/activate
    ```

3.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Set up the database:**
    Configure your database settings in `smartaxi_server/settings.py` (or through environment variables using `dj-database-url`).
    ```bash
    cd smartaxi_server
    python manage.py syncdb
    python manage.py migrate
    ```

5.  **Run the development server:**
    ```bash
    python manage.py runserver 0.0.0.0:8000
    ```

## API Endpoints

The API is mounted under `/api/v1/`.

### Authentication
*   **Api Token**: `/api/v1/token/auth/` (GET) - Retrieves user API key using Basic Auth.
*   **Account**: `/api/v1/account/` (GET) - Retrieves user account details.

### Core Resources
*   **Location**: `/api/v1/location/` (POST) - Record the current location of an authenticated user/taxi.
*   **Mapa (Search)**: `/api/v1/mapa/search/` (GET) - Search for taxis within a specified radius. Parameters: `lat`, `long`, `r`.
*   **Client**: `/api/v1/client/` (GET, POST) - Manage client details.
*   **Client Location**: `/api/v1/clientlocation/` (GET, POST) - Manage client addresses.
*   **Taxi**: `/api/v1/taxi/` (GET, POST, PUT) - Manage taxi profiles and states.
    *   **Taxi Device Registration**: `/api/v1/taxi/<id>/device/` (POST, PUT) - Register a mobile device (GCM) to a taxi.
*   **Push Notifications**: `/api/v1/push/` (GET, POST, PUT) - Manage device registration for notifications.
*   **Notifications**: `/api/v1/notificaciones/` (GET, PUT) - Retrieve or update the status of taxi notifications.
*   **Travel**: `/api/v1/travel/` (GET, POST) - Handle taxi requests and journeys.

## Models Overview

*   **Taxi**: Represents a taxi driver, linked to a User, with status (Available, Working, etc.) and license plate.
*   **Client**: A customer requesting a taxi.
*   **ClientLocation**: Specific location/address coordinates for the client.
*   **Location**: Regular updates of a taxi's location (latitude, longitude, speed).
*   **Notification**: Status messages sent to/from the driver (Created, Sent, Responded, Rejected).

## Signals

The application uses Django signals to trigger push notifications:
*   When a new `Notification` is created, it automatically attempts to send a GCM message to the taxi's registered device.
*   When a driver responds (accepts the request), other pending notifications for the same client are discarded and notified as "already taken".

## License

(Add License info here)
