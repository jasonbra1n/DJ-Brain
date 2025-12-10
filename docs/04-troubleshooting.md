# 4. Troubleshooting

This guide provides solutions to common problems you may encounter while running DJ Brain. The first and most important tool for troubleshooting is viewing the service logs.

## Viewing Service Logs

Almost all issues can be diagnosed by looking at the real-time logs of the Docker containers.

1.  Connect to your device via SSH.
2.  Navigate to the `DJ-Brain` directory.
3.  To view the logs for all running services, use:
    ```bash
    docker-compose logs -f
    ```
4.  To view the logs for a specific service (e.g., `mopidy` or `php_backend`), use:
    ```bash
    docker-compose logs -f <service_name>
    ```
    (Example: `docker-compose logs -f mopidy`)

Press `Ctrl+C` to stop viewing the logs.

---

## Common Issues and Solutions

### Issue: `docker-compose up` fails with an error.

- **Solution 1: Check `docker-compose.yml` syntax.**
  If you have edited the `docker-compose.yml` file, ensure its formatting is correct. YAML is sensitive to indentation.

- **Solution 2: Port conflict.**
  If another service on your host is using a port needed by DJ Brain (like 8080, 6680, or 3306), the container will fail to start. The logs will typically show an "address already in use" error. You will need to either stop the other service or change the port mapping in the `docker-compose.yml` file. For example, change `"8080:80"` to `"8081:80"` to access the DJ Brain frontend on port 8081.

### Issue: Cannot connect to the web interface (e.g., Mopidy or the main frontend).

- **Solution 1: Verify the containers are running.**
  Run `docker-compose ps`. All services should show a state of `Up`. If a service is not running, check its logs (`docker-compose logs -f <service_name>`) to see why it failed.

- **Solution 2: Check your IP address.**
  Ensure you are using the correct IP address for your Raspberry Pi or server.

- **Solution 3: Check firewall rules.**
  If you are running a firewall on your host, ensure it is not blocking the ports used by DJ Brain.

### Issue: The music library is empty or missing songs.

- **Solution 1: Incorrect `MUSIC_PATH` in `.env` file.**
  This is the most common cause. The path to your music library must be the **absolute path** on the host machine. Relative paths (`./music`) or paths from within the container will not work.
  - Correct Linux example: `/mnt/usb-drive/my-music`
  - Correct Windows example: `/c/Users/Me/Music`
  After correcting the path, you must restart the Docker stack (`docker-compose down && docker-compose up -d`) and trigger a rescan if necessary.

- **Solution 2: File permissions.**
  The user running the Docker containers needs to have read access to your music files and folders. If you are having permission issues, ensure your music files are readable by others. A simple (but not ideal for all situations) way to ensure this is:
  ```bash
  sudo chmod -R 755 /path/to/your/music
  ```

- **Solution 3: The initial scan is not complete.**
  If you have a very large library, the first scan can take a long time. Check the Mopidy logs to see if the scan is still in progress.
  ```bash
  docker-compose logs -f mopidy
  ```

### Issue: Changes to code or configuration are not taking effect.

- **Solution: Restart the relevant service.**
  While some services are configured to reload on changes, it is often necessary to restart them to apply new settings. For most changes, restarting the entire stack is the most reliable method.
  ```bash
  # Stop and remove the old containers
  docker-compose down

  # Start fresh with the new configuration
  docker-compose up -d
  ```
