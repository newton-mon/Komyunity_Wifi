# Komyunity_Wifi
A centralized authentication server for network access control.

Project Overview 🌐
This project sets up a robust and scalable RADIUS (Remote Authentication Dial-In User Service) server to manage user authentication for network access. Instead of using a single pre-shared key, it allows for individual user accounts, providing enhanced security and control over who connects to a network. The entire system is built and runs within a WSL2 environment on a Windows host.

Key Components 🛠️
FreeRADIUS: The authentication server that processes connection requests.

PostgreSQL: A powerful relational database used to store user credentials and authentication policies.

WSL2 (Windows Subsystem for Linux): The virtualized environment where the FreeRADIUS server is hosted.

How It Works ⚙️
When a user attempts to connect to a network device (e.g., a Wi-Fi access point or VPN server), the device forwards the user's credentials to the FreeRADIUS server. FreeRADIUS then queries the PostgreSQL database to verify the username and password. Based on the database's response, FreeRADIUS sends an Access-Accept or Access-Reject message back to the network device.

Current Functionality ✨
This repository demonstrates a fully functional RADIUS server configured to:

Run within WSL2.

Successfully connect to and query a PostgreSQL database.

Authenticate users against credentials stored in the radcheck table.
