# docker.solaranzeige
<img src="https://raw.githubusercontent.com/DeBaschdi/solar_config/master/solaranzeige/splash.png" height="100" width="150">

### Prerequisites
You will need to have `docker` installed on your system and the user you want to run it needs to be in the `docker` group.

> **Note:** The image is a multi-arch build providing variants for amd64, arm32v7 and arm64v8 and also, an legacy amd64 Build for old Synology Kernels. - the correct variant for your architecture needs to be tagged e.g :amd64, :arm32v7, :arm64v8, :legacy

## Technical info for docker GUIs (e.g. Synology, UnRaid, OpenMediaVault)
To learn how to manually start the container or about available parameters (you might need for your GUI used) see the following example:

```
docker run \
  -d \
  -e USER_ID="99" \
  -e GROUP_ID="100" \
  -e TIMEZONE="Europe/Berlin" \
  -e UPDATE="yes" \
  -e MOSQUITTO="yes" \
  -e INFLUXDB="yes" \
  -p 3000:3000 \
  -p 1883:1883 \
  -p 8080:80 \
  -v {SOLARANZEIGE_STORAGE}:/solaranzeige \
  -v {INFLUXDB_STORAGE}:/var/lib/influxdb \
  -v {GRAFANA_STORAGE}:/var/lib/grafana \
  -v {PVFORECAST_STORAGE}:/pvforecast \
  -v {WWW_STORAGE}:/var/www \
  --name=Solaranzeige \
  --restart unless-stopped \
  --tmpfs /tmp \
  --tmpfs /var/log \
  takealug/solaranzeige:tag
```

The available parameters in detail:

| Parameter | Optional | Values/Type | Default | Description |
| ---- | --- | --- | --- | --- |
| `USER_ID` | yes | [integer] | 99 | UID to run Solaranzeige as |
| `GROUP_ID` | yes | [integer] | 100 | GID to run Solaranzeige as |
| `TIMEZONE` | yes | [string] | Europe/Berlin | Timezone for the container |
| `-p` | no | [integer] | 3000:3000 | Map Grafana Listenport inside this Container to Host Device Listen Port (Bridge Mode) |
| `-p` | no | [integer] | 1883:1883 | Map Mosquitto Listenport inside this Container to Host Device Listen Port (Bridge Mode) |
| `-p` | no | [integer] | 80:8080 | Map Apache2 Listenport inside this Container to Host Device Listen Port (Bridge Mode) |
| `UPDATE` | yes | yes, no | no | Turn On / Off automatic Update for Solaranzeige each restart inside this Docker |
| `MOSQUITTO` | yes | yes, no | yes | Turn On / Off mosquitto service inside this Container |
| `INFLUXDB` | yes | yes, no | yes | Turn On / Off influxdb service inside this Container |

Frequently used volumes:
 
| Volume | Optional | Description |
| ---- | --- | --- |
| `SOLARANZEIGE_STORAGE` | no | The directory to persist /solaranzeige with Crontab Settings to |
| `INFLUXDB_STORAGE` | no | The directory to to persist /var/lib/influxdb to |
| `GRAFANA_STORAGE` | no | The directory to to persist /var/lib/grafana to |
| `PVFORECAST_STORAGE` | no | The directory to to persist /pvforecast to |
| `WWW_STORAGE` | no | The directory to to persist /var/www to |

When passing volumes please replace the name including the surrounding curly brackets with existing absolute paths with correct permissions.


> **Note:** `INFLUXDB_STORAGE` persist the Database for Grafana. 
> **Note:** `GRAFANA_STORAGE` persist Grafana settings and Dashboards. 
> **Note:** `WWW_STORAGE` persist logfiles and all script-files from solaranzeige.de. 
> **Note:** `PVFORECAST_STORAGE` persist PVForecast data from https://github.com/StefaE/PVForecast
>

## First Run / Initial Setup Solaranzeige
Inside this Container you need to run /solaranzeige/setup 
connect to the Container e.g 
```
docker exec -ti Solaranzeige /solaranzeige/setup
```
> **Note:** For initial Setup instructions see https://solaranzeige.de/phpBB3/viewtopic.php?f=5&t=305
>> Please, after you finish your Initial Setup, restart this Container, and select your Grafana - Dashboard

## Modify Crontab Settings (Needed for Wallbox or Multi controller Configuration)
Just edit your Persist {SOLARANZEIGE_STORAGE}/solaranzeige_cron File and restart your Container
dont forget to add an empty Newline at the End of this File.

## Modify all Settings like Grafana / Influx ect vie Webbrowser 
Open your Webbrowser and open Solaranzeiges "Quick Access" e.g. IP:8080 and use "File Editor" , standard password is "solar" 
after saving, please restart your Container to take effect.

<img src="https://raw.githubusercontent.com/DeBaschdi/docker.solaranzeige/master/Templates/Unraid/pheditor.png" height="379" width="766">

## Support my work
If you like my Work, please [![Paypal Donation Page](https://www.paypalobjects.com/en_US/i/btn/btn_donate_SM.gif)](https://paypal.me/DeBaschdi) - thank you! :-)

## Unraid Template
> **Note:** An Template for Unraid can be found here : https://raw.githubusercontent.com/DeBaschdi/docker.solaranzeige/master/Templates/Unraid/my-Solaranzeige.xml
> Please safe it to into \flash\config\plugins\dockerMan\templates-user, after that you can use this Template in Unraids Webui. Docker > Add Container > Select Template and choose Solaranzeige

<img src="https://raw.githubusercontent.com/DeBaschdi/docker.solaranzeige/master/Templates/Unraid/Screenshot.png" height="325" width="265">

Markup :  - - - -

## QNAP Container Station installation

<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/5362b0bf-bd97-4982-8b82-e9125dec6384" width=600px >
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/ae1085ad-9a1c-49be-a770-1cd9a854eb48" width=600px >

<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/d226496c-666d-4b3b-b2f7-7429b9b4932b" width=400px>

You will find containers but the right container is "takealug/solaranzeige" and click "Bereitstellen".
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/a2f9e8bc-b5b0-4f3b-b4e5-9148ace15dd8" width=500px>
<br />
Accept message with "OK" in new window
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/f3dc7469-4a60-4658-9ec4-3a89e7f57a2d" width=300px>
<br />
Then you must choose a image version "latest"
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/7c0ba884-226e-44fe-90fa-3f6b2a08a7f4" width=300px>
<br />
create container
<br />
> **Note:**  All settings are already set automatically.: 
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/e5b872bf-bb11-415c-a0d3-e34c1046676a" width=450px>
<br />
For the network configuration, there is the option of integrating the container via bridge or nat. I personally prefer my own Bridge with a separate IP that is not the same as the IP address of the QNAP.
<br />

> **Note:**  Please check whether these ports are already occupied on the QNAP.:
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/94816ea7-76c7-43ef-b6a4-05555cb6e58c" width=500px>
<br />

You can have an automatic IP assigned via DHCP or define your own static IP - "Eine statische IP-Adresse verwenden".
> **Note:**  You must define a virtual switch as the interface for this setting.:
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/b4f812d6-c921-4c83-973c-90d3bea3425e" width=500px>
<br />
Then click apply "Übernehmen" and next "Weiter".
<br />
At the end there is a summary where you can check everything. Press Finish "Fertigstellen" 
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/388c2828-48b9-47e7-ae33-57cb1c7cb317" width=500px>
<br />
Container is downloaded and installed. This is then displayed in the Container menu where there is an overview of the installed containers.








### container station terminal

To continue, go to the gear wheel and click on execute.
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/45bf9bf4-d56c-4dcc-969f-6025c4c80fa0" width=600px>
<br />
Then select bin/bash and click execute. 
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/d84945af-3b67-4fe6-9aaf-3f08693cbfe0" width=500px>
<br />
Then a terminal input is displayed
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/e86b106f-bcf3-443a-af60-d9f0d8f8f4da" width=400px>
<br />
type in: 
```
./solaranzeige/setup
```
Now simply go through the installation.

#### per ssh putty or other ssh terminal
Login per putty
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/e3728343-3be4-473c-a2cf-541d69722d9d" width=400px>
<br />
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/050c3874-be64-4cc8-be5f-419e7b55d6f7" width=400px>
<br />

Now you can either log in directly to the container and then run the setup or you can run a direct setup.
container terminal:
```
docker exec -i -t solaranzeige-1 bash
```
```
./solaranzeige/setup
```
or first run
```
docker exec -ti Solaranzeige /solaranzeige/setup
```
<img src="https://github.com/SirRenix/docker.solaranzeige/assets/7069603/7ebb595a-f572-412a-ae59-984b074281d4" width=400px>




