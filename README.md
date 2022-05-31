# pi

Everything related to setting up my Raspberry Pi for Home Automation, based on the book [Control Your Home with Raspberry Pi by Koen Vervloesem](https://koen.vervloesem.eu/books/control-your-home-with-raspberry-pi/)

# Setup

1. Download Imager to prepare the microSD card: https://www.raspberrypi.com/software
2. Flash Raspberry Pi OS (64-bit) Lite to the SD card
   - Go to the advanced settings
   - Set a hostname and enable SSH with password
   - Set a password, but keep `pi` as the username. Save this password to your password manager immediately.
   - Set locale settings
   - Disable Telemetry (Analytics)
3. Insert microSD card in the Raspberry Pi
4. Connect Raspberry Pi to ethernet
5. Connect the Raspberry Pi to power, it takes a while the first time, because it expands the file system to the microSD cards full capacity.
6. Check DHCP leases on Routers web interface and find the IP address of the Raspberry Pi. MAC address of every Raspberry Pi starts with `b8:27:eb` (the older models) or `dc:a6:32` (beginning from the Raspberry Pi 4).
7. Give Raspberry Pi a static IP address in the Routers web interface. The menu item should be called something similar to "DHCP static mappings".
8. Check DHCP leases again to get Raspberry Pi's IP address.
9. Connect via SSH `ssh pi@IPADDRESS`.
10. After accepting SSH fingerprint, use the password you set with the imager tool.
11. Update package repositories and upgrade packages:

```
sudo apt update
sudo apt upgrade
```

15. Configure extra settings with `sudo raspi-config`.

- Set locale settings
- In performance options, change the memory split so the GPU gets the minimal value of 16 MB, since we don't need a graphical interface.

16. If asked to reboot, press Yes.
17. Install Pip `sudo apt install python3-pip`.
18. Install virtual environment functionality `sudo apt install python3-venv`.
19. Install docker with `curl -sSL https://get.docker.com | sh`.
20. Give pi user access to Docker `sudo usermod pi -aG docker`. Otherwise you need to prefix Docker commands with `sudo`.
21. Reboot with `sudo reboot`.
22. Check to see that the Docker engine is running: ` systemctl status docker`.
23. Try the hello-world container `docker run --rm hello-world`.
24. Install git `sudo apt-get install git`.
25. Generate SSH key with: `ssh-keygen -t ed25519 -C "your_email@example.com"`
26. Add the SSH key to the SHH agent: `ssh-add ~/.ssh/id_ed25519`
27. Copy public key to Github: `cat ~/.ssh/id_ed25519.pub`
28. Use this repo as the home directory:
    1.  `cd ~`
    2.  `git init .`
    3.  `git remote add origin git@github.com:mathiassoeholm/pi.git`
    4.  `git checkout main`
    5.  `git pull`
29. Install Neovim `sudo apt-get install neovim`
30.

31. Create a directory for the Docker containers `mkdir /home/pi/containers`.
32. Install Docker Compose `sudo pip3 install docker-compose`.
33. Check to see if Docker Compose installed correctly `docker-compose --version`.
34. Ask for password on `sudo`
    1. `sudo visudo /etc/sudoers.d/010_pi-nopasswd`
    2. Change `pi ALL=(ALL) NOPASSWD: ALL` to `pi ALL=(ALL) PASSWD: ALL`
35. To use TLS, we need to set up a local domain in the router settings. Set `raspberrypi.home` as a domain for the IP Address of the Raspberry Pi.
36. Install `arm` release of `mkcert` with:

```sh
curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/arm"
chmod +x mkcert-v*-linux-arm
sudo cp mkcert-v*-linux-arm /usr/local/bin/mkcert
```

29. Check if `mkcert` is installed correctly with `mkcert --help`.
30. Create a root certificate (CA) with `mkcert -install`. This can take a while.
31. Copy root certificate to desktop computer with `scp pi@IP:.local/share/mkcert/rootCA.pem .`. Replace IP with IP Address of Raspberry Pi. The root certificate can then be added to your browser.
32. Create a TLS certificate

## Python virtual environment

- Create with `python3 -m venv .venv`
- Activate with `source .venv/bin/activate`
- Install package requirements with `pip3 install -r requirements.txt`

## Docker

### Example `docker-compose.yml`

```yml
version: '3.7'

services: hello-world:
  image: python:3.8-alpine
  ports:
    - "80:8000"
  command: "python -m http.server 8000"
```

Check syntax with `docker-compose config -q`.

Lint with `sudo apt install yamllint`, then `yamllint docker-compose.yml`.

Starting containers: `docker-compose up -d`.

Take container down: `docker-compose down`.

List running containers `docker ps`
