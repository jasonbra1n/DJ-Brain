# 1. Installation Guide

This guide provides step-by-step instructions for setting up DJ Brain on a dedicated host, such as a Raspberry Pi.

## Hardware Requirements

While DJ Brain can run on various Linux-based servers, it is optimized for low-power devices.

- **Recommended Host:** Raspberry Pi 4B (2GB or more) or a newer model. This is sufficient for all core jukebox features. A more powerful machine (like a mini-PC) is recommended if you plan to use the advanced AI features in the future.
- **Storage:** A high-quality microSD card (32GB or larger, "Endurance" or "A2" rated is recommended).
- **Power Supply:** An official Raspberry Pi USB-C power adapter.

---

## Step 1: Prepare the Raspberry Pi

These steps will guide you through setting up a fresh, headless (no monitor attached) Raspberry Pi.

### 1.1 Install Raspberry Pi OS

We will use the official **Raspberry Pi Imager** tool, as it simplifies the process of pre-configuring the device for network access.

1.  Download and install the [Raspberry Pi Imager](https://www.raspberrypi.com/software/) on your computer.
2.  Insert your microSD card into your computer.
3.  Open the Raspberry Pi Imager.
    -   **Choose Device:** Select your Raspberry Pi model (e.g., Pi 4).
    -   **Choose OS:** Select `Raspberry Pi OS (other)` -> `Raspberry Pi OS Lite (64-bit)`. A "Lite" OS is ideal as we don't need a desktop environment.
    -   **Choose Storage:** Select your microSD card.
4.  Click **Next**, then **Edit Settings** to pre-configure your Pi:
    -   Set a **hostname** (e.g., `djbrain`).
    -   Set a **username** and a secure **password**.
    -   Check the box to **Configure wireless LAN** and enter your Wi-Fi credentials.
    -   Under the **Services** tab, check the box to **Enable SSH**.
5.  Click **Save**, then **Yes** to confirm, and finally **Write** to flash the OS to the card.

### 1.2 First Boot and System Update

1.  Safely eject the microSD card from your computer and insert it into your Raspberry Pi.
2.  Power on the Pi. Wait a few minutes for it to boot and connect to your network.
3.  Find your Pi's IP address from your router's admin page.
4.  Connect to your Pi using an SSH client (like Terminal on macOS/Linux or PuTTY on Windows):
    ```bash
    ssh your_username@<your_pi_ip_address>
    ```
5.  Once connected, update the system's package lists and upgrade all installed packages. This is a critical step for security and stability.
    ```bash
    sudo apt update
    sudo apt full-upgrade -y
    ```

---

## Step 2: Install Docker and Git

DJ Brain runs in Docker containers, which makes installation clean and simple.

1.  **Install Docker:** We will use the official convenience script.
    ```bash
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    ```
2.  **Add your user to the `docker` group.** This allows you to run Docker commands without `sudo`.
    ```bash
    sudo usermod -aG docker $USER
    ```
    **Important:** You must log out and log back in for this change to take effect. Close your SSH session and reconnect.

3.  **Install Git:** We need `git` to download the DJ Brain software.
    ```bash
    sudo apt install git -y
    ```

Your Raspberry Pi is now fully prepared. The next step will be to download and configure the DJ Brain software itself.

---

## Step 3: Download and Configure DJ Brain

Now we will download the DJ Brain software and set up its configuration.

1.  **Clone the Repository:**
    Clone the DJ Brain repository from GitHub to your home directory.
    ```bash
    cd ~
    git clone https://github.com/jasonbra1n/DJ-Brain.git
    cd DJ-Brain
    ```

2.  **Create Your Configuration File:**
    The project uses a `.env` file to store sensitive information and environment-specific settings. An example file is provided.
    ```bash
    cp .env.example .env
    ```

3.  **Edit the Configuration:**
    Open the `.env` file with a text editor (like `nano`) to set your music path and secure passwords.
    ```bash
    nano .env
    ```
    -   **`DB_ROOT_PASSWORD`**: Change this to a secure, private password for the database administrator.
    -   **`DB_PASSWORD`**: Change this to a different secure password for the main application user.
    -   **`MUSIC_PATH`**: **This is the most critical setting.** You must change `/path/to/your/music` to the real, absolute path where your music files are stored on the Raspberry Pi.

    Press `Ctrl+X`, then `Y`, then `Enter` to save the file and exit `nano`.

---

## Step 4: Launch the System

With the configuration in place, you are ready to start the entire DJ Brain system.

1.  **Run Docker Compose:**
    From within the `DJ-Brain` directory, run the following command. This will download the necessary Docker images and start all the services. The first launch may take several minutes.
    ```bash
    docker-compose up -d
    ```
    The `-d` flag runs the containers in "detached" mode, meaning they will continue to run in the background.

2.  **Initial Music Scan:**
    The very first time you start the system, Mopidy (the music server) needs to scan your entire music library. This process can take a long time if your library is large. You can watch the progress of the scan by viewing the logs:
    ```bash
    docker-compose logs -f mopidy
    ```
    You will see output from Mopidy. Look for messages related to scanning your library. Once the scan is complete, you can safely exit the log view by pressing `Ctrl+C`.

---

## Step 5: Verify the Installation

DJ Brain should now be running. You can verify this by accessing its components through your web browser.

1.  **Mopidy Web Interface:**
    Mopidy has its own web client, which is useful for direct administration.
    -   **URL:** `http://<your_pi_ip_address>:6680/`
    -   You should see the Mopidy landing page. From here you can access various web clients.

2.  **DJ Brain Frontend (Once Available):**
    The main DJ Brain web interface will be served from the PHP container.
    -   **URL:** `http://<your_pi_ip_address>:8080/`
    -   **Note:** In the early phases of development, this interface may not be fully functional. However, accessing the URL should not result in a "Connection Refused" error if the container is running correctly.

Your installation is now complete. The system will automatically restart if you reboot your Raspberry Pi. You can now proceed to the **[Configuration](./02-configuration.md)** guide to learn more about managing your new jukebox.