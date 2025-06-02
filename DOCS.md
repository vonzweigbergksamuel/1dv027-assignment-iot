# Documentation

## Relevant links

[Dashboard URL](https://cscloud6-32.lnu.se/grafana/d/a8ea1733-d09f-41ca-ad64-631d8e55b277/samuels-dashboard?orgId=1&from=now-15m&to=now&timezone=browser&refresh=10s)

<br>

Login credentials for the dashboard:
| Username | Password |
|-------------|-------------|
| `lnu` | iot2025lnu |

<br>

## Configuration files

[Telegraf configuration file](/telegraf/telegraf.conf)

[Docker Compose file](compose.yml)

[Example Env file](example.env)

[Grafana Dashboard Export File](grafana-export.json)

<br>

## Setup process

I went with Docker for this project because it makes everything super easy to set up. To get started, you just need to copy the `example.env` file to `.env` and fill in some basic settings like the InfluxDB username and password.

The cool thing about using Docker is that everything runs in containers. Just run `docker compose up -d` and it fires up all three main parts: InfluxDB (which stores all the sensor data), Telegraf (which collects the data), and Grafana (which makes the nice dashboards).

I set up Telegraf to connect to our MQTT broker where all the sensor data comes in. It's pretty simple - it just listens for the sensor readings and sends them to InfluxDB every 10 seconds. If you want to change any settings, just edit the `telegraf.conf` file.

For the dashboard part, I used Grafana. Once everything is running, you can open it in your browser and import the dashboard I made. It shows all the sensor data in different ways - you've got charts for temperature, gauges for pressure and humidity, and graphs for air quality.

<br>

## Data Flow

Here's how the data moves through the system: First, the sensors measure stuff like temperature and humidity and send it to the MQTT broker. They package everything in JSON format, which makes it easy to work with.

Telegraf picks up this data from the MQTT broker. It's basically just sitting there watching for new sensor readings to come in every 10 seconds. I set it up this way so we get regular updates without overloading the system.

The data then goes into InfluxDB, which is perfect for this kind of time-based data. It's pretty smart about how it organizes everything - each reading gets stored with tags that tell us where it came from and what it's measuring.

Finally, Grafana grabs the data from InfluxDB and shows it in the dashboard. I set it up to update in real-time and show the data in ways that make sense - like using bar charts for temperature trends and gauges for current readings. You can see both what's happening right now and how things have changed over time.

<br>

## Time-Series Optimizations

The nice thing about using these tools is that they handle most of the optimizations automatically. I just set up a 10-second interval for collecting the data in Telegraf, and the rest happens behind the scenes.

InfluxDB does all the heavy lifting when it comes to storing the time-series data. Since it's built specifically for this kind of data, it automatically handles things like data compression, efficient storage, and quick access to the readings. I didn't have to worry about any of that stuff.

Grafana is also pretty smart about handling the data visualization. It automatically figures out the best way to display the data based on the time range you're looking at. So if you're viewing a day's worth of data, it'll automatically aggregate it to keep the graphs running smooth, but you can still zoom in to see more detail when you need to.

