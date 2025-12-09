# Contributing to DJ Brain

First off, thank you for considering contributing to DJ Brain! Your help is essential for keeping this project great.

## 💻 Development Environment Setup

The entire project is orchestrated with Docker, which simplifies setup significantly.

1.  **Prerequisites:**
    -   [Git](https://git-scm.com/)
    -   [Docker](https://www.docker.com/products/docker-desktop/)

2.  **Fork & Clone:**
    -   Fork the repository on GitHub.
    -   Clone your fork locally: `git clone https://github.com/YOUR-USERNAME/DJ-Brain.git`

3.  **Configuration:**
    -   The `docker-compose.yml` file uses environment variables for configuration. Create a `.env` file from the example:
        ```bash
        cp .env.example .env
        ```
    -   **Edit `.env`** to set your music path and secure passwords. The `php_backend` service volume is already mounted to `src/api`, so any changes you make there will be reflected live inside the container.

4.  **Run the Stack:**
    ```bash
    docker-compose up
    ```
    (Use the `-d` flag to run in detached mode).

## 📂 Project Structure

```text
.
├── .env.example        # Example environment variables for Docker
├── ARCHITECTURE.md     # High-level technical overview and design choices
├── CHANGELOG.md        # Log of all notable changes
├── CONTRIBUTING.md     # This file: guide for developers
├── LICENSE             # The MIT open-source license
├── README.md           # The main project entry point and quick start
├── project_plan.md     # The detailed project roadmap
├── docs/               # User-facing documentation and manual
├── config/             # Persistent configuration files (e.g., mopidy.conf)
├── data/               # Persistent data (e.g., MariaDB database files)
├── src/
│   ├── api/            # The PHP Slim backend application
│   ├── brain/          # The Python AI DJ engine
│   └── frontend/       # The Vanilla JS/HTML/CSS frontend
└── docker-compose.yml  # The master file for orchestrating all services
```

## 📚 Key Documents

Before you start coding, it's helpful to familiarize yourself with the project's design and goals.
- **[Project Plan](project_plan.md):** Understand the roadmap and current phase of development.
- **[Architecture Overview](ARCHITECTURE.md):** Learn about the high-level design, service interactions, and commercial strategy.
- **[API Documentation](API_DOCUMENTATION.md):** See the contract for how the frontend and backend communicate.
- **[User Manual](./docs/index.md):** Review the end-user documentation. As you add or change features, please update the relevant sections of the manual.

## 🤝 How to Contribute

1.  Create a new branch for your feature or bugfix: `git checkout -b feature/my-awesome-feature`.
2.  Write your code! Remember to update the user manual in the `docs/` directory if you are adding or changing functionality.
3.  Ensure your code adheres to the project's general style.
4.  **Test your changes.** Before submitting, ensure your feature works as expected and does not introduce any new bugs. This is a critical step to verify a roadmap accomplishment is complete and correct.
5.  Update the `CHANGELOG.md` with any significant changes under the `[Unreleased]` section.
6.  Submit a Pull Request to the `main` branch of the original repository.

## 🐛 Reporting Bugs

Please open an issue on GitHub and include as much detail as possible, including steps to reproduce, logs from `docker-compose logs`, and expected vs. actual behavior.