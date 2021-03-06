# Install Blynk Server on Raspberry Pi

[Quick install](https://github.com/librarysteve/installblynkpi#quick-install)

[Manual install](https://github.com/librarysteve/installblynkpi#manual-install)

[Access the admin pannel](https://github.com/librarysteve/installblynkpi#to-access-the-admin-pannel)

[Uninstall info](https://github.com/librarysteve/installblynkpi/blob/master/README.md#uninstall)

This script does the following:
  * Update and Upgrade
  * Installs OpenJDK 8
  * Downloads and initializes the Blynk Server
  * Installs the server as a systemd service if you so choose

## Quick Install

1) Clone this repo

```shell
git clone https://github.com/librarysteve/installblynkpi.git
```
2) Run the installer script
```shell
cd ./installblynkpi
```
```shell
sudo chmod +x ./install.sh
```
```shell
sudo ./install.sh
```
3) Answer "Yes" if you would like it to start on boot

## Manual Install 
1) Update, upgrade, and install openjdk 8
```shell
sudo apt-get update && sudo apt-get -y upgrade
```
```shell
sudo apt-get install -y openjdk-8-jdk
```
2) Download a copy of the latest blynk server from [The Blynk Server Repo](https://github.com/blynkkk/blynk-server/releases)
or via wget
```shell
wget https://github.com/blynkkk/blynk-server/releases/download/v0.41.10/server-0.41.10-java8.jar
```
3) Create a folder for everything Blynk Server related
```shell
mkdir blynk_server
```
4) Make a folder for server data
```shell
mkdir ./blynk_server/server_data
```
5) Create a shell script to start the server
something like:
```shell
#!/bin/bash
$(java -jar /home/pi/blynk_server/server-0.41.10-java8.jar -dataFolder /home/pi/blynk_server/data_folder)
```
6) Create a service "Unit File" so the server will start on boot using systemd 
```shell
sudo nano /etc/systemd/system/blynk.service
```
__Example Unit File:__
```
[Unit]
Description=Blynk IoT Server
After=network.target

[Service]
ExecStart=/home/pi/blynk_server/start_blynk_server.sh
WorkingDirectory=/home/pi/blynk_server
StandardOutput=inherit
StandardError=inherit
Restart=Always
User=pi

[Install]
WantedBy=multi-user.target
```
7) Enable the service with systemctl
First start the service
```shell
sudo systemctl start blynk.service
```
Then enable
```shell
sudo systemctl enable blynk.service
```
Then check to see if it is running
```shell
sudo systemctl status blynk
```
*if you see an "active" message (usually in green) you're good to go*

## To access the admin pannel

1) Browse to https://(RPi IP ADDRESS):9443

2) Login with default credentials
  User/Email:admin@blynk.cc
  Password:admin
__NOTE: Unless you have an email server running, there will be no email sent. That means no password reset__ 

## Uninstall
1) Make the uninstall script executable
```shell
chmod +x ./uninstall.sh
```
2) Run the uninstaller and follow the prompt
