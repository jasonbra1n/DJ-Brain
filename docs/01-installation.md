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