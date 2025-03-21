## About 

Hacking the Cambridge Audio AXN10 Streamer.

Url format is `ip-address/smoip/endpoint`. My guess is `SMOIP` stands for Stream Magic Over IP, a play on `VOIP`.

## Endpoints
### system/info

`http://192.168.XX.XX/smoip/system/info`

```
{
  "data": {
    "name": "AXN10",
    "timezone": "Europe/London",
    "locale": "en_GB",
    "usage_reports": true,
    "setup": true,
    "sources_setup": true,
    "versions": [
      {
        "component": "arc",
        "version": ""
      },
      {
        "component": "M4",
        "version": "1.0+gitAUTOINC+0a0e6baee3"
      },
      {
        "component": "cast",
        "version": "1.52.272222"
      },
      {
        "component": "application",
        "version": "1.0+gitAUTOINC+9979df4655"
      },
      {
        "component": "service-pack",
        "version": "v132-b-007"
      }
    ],
    "udn": "e2ba4eea-d744-4621-8234-d5b469b43c23",
    "hcv": 4005,
    "board_id": "08080",
    "model": "AXN10",
    "unit_id": "0040553f",
    "max_http_body_size": 65536,
    "api": "1.6",
    "cpu": {
      "bus_freq": "high",
      "temp": 37000,
      "cores": [
        {
          "load": 0,
          "freq": 200000
        },
        {
          "load": 3,
          "freq": 200000
        },
        {
          "load": 1,
          "freq": 200000
        },
        {
          "load": 2,
          "freq": 200000
        }
      ]
    }
  }
}
```

### system/power

`http://192.168.XX.XX/smoip/system/power`

```
{
  "data": {
    "power": "ON",
    "standby_mode": "NETWORK",
    "auto_power_down": 1800
  }
}
```
### system/sources

`http://192.168.XX.XX/smoip/system/sources`

```
{
  "data": {
    "sources": [
      {
        "id": "IR",
        "name": "Internet Radio",
        "default_name": "Internet Radio",
        "class": "stream.radio",
        "nameable": false,
        "ui_selectable": true,
        "description": "Internet Radio",
        "description_locale": "Internet Radio",
        "preferred_order": 7
      },
      {
        "id": "MEDIA_PLAYER",
        "name": "Media Library",
        "default_name": "Media Library",
        "class": "stream.media",
        "nameable": false,
        "ui_selectable": true,
        "description": "Media Player",
        "description_locale": "Media Player",
        "preferred_order": 8
      },
      {
        "id": "AIRPLAY",
        "name": "AirPlay",
        "default_name": "AirPlay",
        "class": "stream.service.airplay",
        "nameable": false,
        "ui_selectable": true,
        "description": "AirPlay",
        "description_locale": "AirPlay",
        "preferred_order": 2
      },
      {
        "id": "SPOTIFY",
        "name": "Spotify",
        "default_name": "Spotify",
        "class": "stream.service.spotify",
        "nameable": false,
        "ui_selectable": false,
        "description": "Spotify",
        "description_locale": "Spotify",
        "preferred_order": 4
      },
      {
        "id": "BLUETOOTH",
        "name": "Bluetooth",
        "default_name": "Bluetooth",
        "class": "bluetooth",
        "nameable": false,
        "ui_selectable": true,
        "description": "Bluetooth",
        "description_locale": "Bluetooth",
        "preferred_order": 1
      },
      {
        "id": "CAST",
        "name": "Chromecast built-in",
        "default_name": "Chromecast built-in",
        "class": "stream.service.cast",
        "nameable": false,
        "ui_selectable": false,
        "description": "Chromecast built-in",
        "description_locale": "Chromecast built-in",
        "preferred_order": 6
      },
      {
        "id": "ROON",
        "name": "Roon Ready",
        "default_name": "Roon Ready",
        "class": "stream.service.roon",
        "nameable": false,
        "ui_selectable": false,
        "description": "Roon Ready",
        "description_locale": "Roon Ready",
        "preferred_order": 3
      },
      {
        "id": "TIDAL",
        "name": "TIDAL Connect",
        "default_name": "TIDAL Connect",
        "class": "stream.service.tidal",
        "nameable": false,
        "ui_selectable": false,
        "description": "TIDAL",
        "description_locale": "TIDAL",
        "preferred_order": 5
      }
    ]
  }
}
```

### system/sources/USB_AUDIO

`http://192.168.XX.XX/smoip/system/sources/USB_AUDIO`

```
{
  "code": 114,
  "message": "'USB_AUDIO' not available"
}
```

### system/upnp

`http://192.168.XX.XX/smoip/system/upnp`

```
{
  "data": {
    "devices": [
      {
        "model": "AXN10",
        "name": "AXN10",
        "manufacturer": "Cambridge Audio",
        "udn": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
        "description_url": "http://192.168.XX.XX:8050/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/description.xml"
      }
    ]
  }
}
```

UPnP Description XML: `http://192.168.XX.XX:8050/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/description.xml`
### presets/list

`http://192.168.XX.XX/smoip/presets/list`

```
{
  "data": {
    "start": 1,
    "end": 99,
    "max_presets": 99,
    "presettable": false,
    "presets": [
      {
        "id": 1,
        "name": "Netil",
        "type": "Stream",
        "class": "stream.radio",
        "state": "OK",
        "is_playing": false
      },
      {
        "id": 2,
        "name": "NTS Radio",
        "type": "Radio",
        "class": "stream.radio",
        "state": "OK",
        "is_playing": false,
        "art_url": "https://static.airable.io/91/52/156717.png",
        "airable_radio_id": 2503123992588371
      },
      {
        "id": 3,
        "name": "NTS Radio 2",
        "type": "Radio",
        "class": "stream.radio",
        "state": "OK",
        "is_playing": false,
        "art_url": "https://static.airable.io/68/08/783550.png",
        "airable_radio_id": 4650421128142625
      }
    ]
  }
}
```

### info/spec

`http://192.168.XX.XX/smoip/system/info/spec`

```
{
  "data": {
    "timezone": {
      "enum": [
      ...
      ]
    },
    "locale": {
      "enum": [
      ...
      ]
    }
  }
}
```

### system/services

`http://192.168.XX.XX/smoip/system/services`

```
{
  "data": {
    "services": [
      {
        "service_id": "qobuz",
        "supported": true,
        "app_visible": false,
        "version": 2
      },
      {
        "service_id": "tidal",
        "supported": true,
        "app_visible": false,
        "version": 2
      },
      {
        "service_id": "deezer",
        "supported": true,
        "app_visible": false,
        "version": 1
      }
    ],
    "qobuz": true,
    "tidal": true,
    "deezer": true
  }
}
```

### system/sources/CAST

`http://192.168.XX.XX/smoip/system/sources/CAST`

```
{
  "data": {
    "cast_setup_state": "enabled",
    "tutorials_seen": true,
    "share_usage_data": true
  }
}
```

### system/update

`http://192.168.XX.XX/smoip/system/update`

```
{
  "data": {
    "early_update": false,
    "update_available": false,
    "updating": false
  }
}
```

### queue/info

`http://192.168.XX.XX/smoip/queue/info`

```
{
  "data": {
    "total": 0,
    "id_array_token": 0
  }
}
```

### Control

### zone/play_control

`http://192.168.XX.XX/smoip/zone/play_control`

```
@Query("zone") String default: ZONE1
@Query("action") play, stop, toggle, pause, skip, seek, shuffle, repeat
```

`http://192.168.XX.XX/smoip/stream/radio?zone=ZONE1&action=pause`
### stream/radio

`http://192.168.XX.XX/smoip/stream/radio`

```
@Query("zone") String default: ZONE1
@Query("name") String 
@Query("url") String
```

`http://192.168.XX.XX/smoip/stream/radio?zone=ZONE1&name=Netil&url=https://netilradio.out.airtime.pro/netilradio_a`

No Json response on success, just a 200.

## zone/state

`http://192.168.XX.XX/smoip/zone/state`

```
@Query("zone") String
@Query("volume_percent_change") Int
```

`http://192.168.XX.XX/smoip/zone/state`

```
@Query("zone") String
@Query("volume_step_change") Int
```
## KotlinScript CLI

See [Kotlin script](../computers/Kotlin%20script.md) for setup notes, filename is `cacli.main.kts` - the _.main_ is important, `cacli` is Cambridge Audio CLI. You need your device IP address. eg. `./cacli.main.kts 192.168.XX.XX toggle` to toggle streaming radio from play <> pause.

```
#!/usr/bin/env kotlin  
  
import java.net.URL  
import java.net.HttpURLConnection  
  
fun String.get(): String? {  
   val url = this  
   with(URL(url).openConnection() as HttpURLConnection) {  
      log("GET $url")  
      log("Response Code: $responseCode")  
      val sb = StringBuilder()  
      inputStream.bufferedReader().use {  
         it.lines().forEach { line ->  
            sb.append("$line\n")  
         }  
      }  
      return sb.toString()  
   }  
}  
  
fun info(ip: String){  
   log("info()")  
   "http://$ip/smoip/system/info".get()  
}  
  
fun pause(ip: String){  
   log("pause()")  
   "http://$ip/smoip/zone/play_control?zone=ZONE1&action=pause".get()  
}  
  
fun play(ip: String, streamUrl: String?){  
   log("play()")  
   "http://$ip/smoip/stream/radio?zone=ZONE1&name=Radio&url=$streamUrl".get()  
}  
  
fun stop(ip: String){  
   log("stop()")  
   "http://$ip/smoip/zone/play_control?zone=ZONE1&action=stop".get()  
}  
  
fun toggle(ip: String){  
   log("stop()")  
   "http://$ip/smoip/zone/play_control?zone=ZONE1&action=toggle".get()  
}  
  
fun log(message: String){  
   println(message)  
}  
  
log("\nCambridge Audio Streamer CLI\n")  
  
if(args.isNotEmpty()) {  
   when (args.size) {  
      1 -> info(args[0])  
      else -> when (args[1]) {  
         "stop" -> stop(args[0])  
         "pause" -> pause(args[0])  
         "toggle" -> toggle(args[0])  
         "play" -> when {  
            args.size < 3 -> log("Play requires a stream url: ./cacli.main.kts ipAddress play streamUrl")  
            else -> play(args[0], args[2])  
         }  
      }  
   }  
}else{  
   log("No commands")  
}
```