# Deemix.Autoloader

Deemix Autoloader manages your artists and playlists completely automated. You add your favorite artists and playlists, and Deemix Autoloader does the rest for you.

[![Discord: https://discord.gg/Zxc9hcU](https://img.shields.io/discord/751788862644158504?color=blue&label=Discord&style=for-the-badge)](https://discord.gg/Zxc9hcU)
[![GitHub: Last commit date (develop)](https://img.shields.io/github/last-commit/TheUltimateC0der/Deemix.Autoloader/develop.svg?style=for-the-badge&colorB=177DC1)](https://github.com/TheUltimateC0der/Deemix.Autoloader/commits/develop)
<br/>
[![Docker Automated build: theultimatecoder/deemix.autoloader](https://img.shields.io/docker/cloud/automated/theultimatecoder/deemix.autoloader?color=blue&style=for-the-badge)](https://hub.docker.com/r/theultimatecoder/deemix.autoloader)
[![Docker Build Status: theultimatecoder/deemix.autoloader](https://img.shields.io/docker/cloud/build/theultimatecoder/deemix.autoloader?color=blue&style=for-the-badge)](https://hub.docker.com/r/theultimatecoder/deemix.autoloader)
[![Docker Pulls: theultimatecoder/deemix.autoloader](https://img.shields.io/docker/pulls/theultimatecoder/deemix.autoloader?color=blue&style=for-the-badge)](https://hub.docker.com/r/theultimatecoder/deemix.autoloader)



### What Deemix Autoloader does for you

- Manage your Artists
- Manage your Playlists
- Automatically updates your playlists
- Automatically updates your artists
- Calculates the size of your media


### How to spin-up your own instance

#### Docker-Compose

```
version: "3"
services:
    deemix-autoloader:
        image: "theultimatecoder/deemix.autoloader:master"
        environment:
            # Connectionstring for the database
            ConnectionStrings__DefaultConnection: "server=mssql;uid=sa;pwd=H^yi4HtSY$rgd@ptd9PD6YN#dJni6HsNnG^kouXB62zcd4jQKAyw3hp3HcCA7Zp2qco6R&!oC%YzCV#!B5r@tWZerb6KB3NywiCzbeVy#Z6m#q6$Dq4WgFb2!o%vLV^T;database=Deemix.Autoloader;pooling=true"
            # Hangfire dashboard
            Hangfire__DashboardPath: "/autoloaderjobs"
            Hangfire__Password: "p2S3cVY6Yojkby9PYG3AbGPqVzbo8KLS"
            Hangfire__Username: "Autoloader"
            Hangfire__Workers: "2"
            # Configure the cron expression for your jobs
            JobConfiguration__GetUpdatesRecurringJob: "15 * * * *"
            JobConfiguration__SizeCalculatorRecurringJob: "* 6 * * *"
            Kestrel__EndPoints__Http__Url: "http://0.0.0.0:5555"
            # Use the id command in your shell to determine the ids
            PGID: "1234"
            PUID: "1234"
        ports:
            - "5555:80"
        depends_on:
            - mssql
        volumes:
            # Mount the deemix config files
            /opt/deemix-autoloader/deemix:/config/.config/deemix
            # Mount your media folder
            /mnt/unionfs:/mnt/unionfs
    mssql:
        image: "mcr.microsoft.com/mssql/server:2019-latest"
        environment:
            SA_PASSWORD: "H^yi4HtSY$rgd@ptd9PD6YN#dJni6HsNnG^kouXB62zcd4jQKAyw3hp3HcCA7Zp2qco6R&!oC%YzCV#!B5r@tWZerb6KB3NywiCzbeVy#Z6m#q6$Dq4WgFb2!o%vLV^T"
            ACCEPT_EULA: "Y"
        volumes:
            # Persist the db files
            /opt/deemix-autoloader/mssql:/var/opt/mssql
```