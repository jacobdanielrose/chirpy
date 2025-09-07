# Chirpy

Chirpy is a microblogging platform, inspired by Twitter, that allows users to register, log in, and post short messages called "chirps." The platform is built with Go and provides a RESTful API for both frontend and programmatic access.

---

## Features

- **User Management:** Register, log in, update profile, and (with Chirpy Red) unlock more features.
- **Chirp Posting:** Create, fetch, and delete short messages ("chirps").
- **Authentication:** JWT-based authentication for secure API access.
- **Chirp Validation:** Built-in endpoint for chirp content validation.
- **Admin & Metrics:** Health checks, metrics, and database reset endpoint (dev only).
- **Webhooks:** Integration endpoint for external events (e.g., user upgrade via Polka).
- **REST API:** Well-documented API with JSON input/output.

---

## Getting Started

### Prerequisites

- Go 1.21 or higher
- PostgreSQL database
- (optional) Docker for containerization

### Setup

1. **Clone the repository:**
   ```sh
   git clone https://github.com/jacobdanielrose/chirpy.git
   cd chirpy
   ```

2. **Set up environment variables:**  
   Copy `.env.example` (if present) or set the following variables:
   ```
   DB_URL=postgres://user:password@localhost:5432/chirpydb?sslmode=disable
   PLATFORM=dev
   JWT_SECRET=your_jwt_secret
   POLKA_KEY=your_polka_api_key
   ```

3. **Install dependencies:**
   ```sh
   go mod download
   ```

4. **Run database migrations (if required):**
   - Use your preferred migration tool or check project docs for migration scripts.

5. **Run the server:**
   ```sh
   go run main.go
   ```

6. **Frontend:**  
   Chirpy serves frontend files from `/app/` and static assets from `/assets/`.

---

## API Documentation

See full API details, endpoints, and example requests/responses in  
**[docs/API_DOCUMENTATION.md](docs/API_DOCUMENTATION.md)**

---

## Folder Structure

```
.
├── main.go
├── handler_chirps.go
├── handler_users.go
├── handler_validate.go
├── handler_refresh.go
├── handler_webhooks.go
├── internal/
├── assets/
├── docs/
│   └── API_DOCUMENTATION.md
└── ...
```

---

## Contributing

Pull requests and issues are welcome!  
Please open an issue first if you want to discuss a new feature or bug.

---

## License

MIT License

---

## Acknowledgements

- Inspired by Twitter's core features.
- Built with Go and PostgreSQL.

---

For more details, see the [API Documentation](docs/API_DOCUMENTATION.md).