# lovelace-hass-arlo

## Version 0.2

**Be warned, 0.2 is in alpha**

It's working for me, but it's very alpha so be prepared to return to
version 0.1 if things go wrong.

I've put it out there so people can try it if they want. The underlying
architecture is very different and (I hope) a lot more efficient.

The `library_sizes` config is a good place to start.

The card now supports localisation but only English is provided at the
moment. If anybody fancies translating look at `en.js`
[here](https://github.com/twrecked/lovelace-hass-aarlo/tree/master/lang), you
just need to translate the strings.


## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Configuration](#configuration)
- [Further Documentation](#further-documentation)
- [How it looks](#how-it-looks)


<a name="introduction"></a>
## Introduction
Lovelace card designed specifically for the [AArlo
Integration](https://github.com/twrecked/hass-aarlo).

<a name="introduction-features"></a>
#### Features
It provides:
* Motion and sound notifications.
* Access to the camera library recordings.
* Live streaming.
* Support for doorbell and door opening notifications.
* Support for toggling lights during streaming.

<a name="introduction-notes"></a>
#### Notes
This document assumes you are familiar with Home Assistant setup and configuration.

<a name="introduction-thanks"></a>
#### Thanks
Many thanks to:
* [Button Card](https://github.com/kuuji/button-card/blob/master/button-card.js)
  for a working lovelace card I could understand
* [JetBrains](https://www.jetbrains.com/?from=hass-aarlo) for the excellent
  **PyCharm IDE** and providing me with an open source license to speed up the
  project development.

  [![JetBrains](/images/jetbrains.svg)](https://www.jetbrains.com/?from=hass-aarlo)


<a name="installation"></a>
## Installation

<a name="installation-hacs"></a>
#### HACS
[![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=for-the-badge)](https://github.com/custom-components/hacs)

Aarlo is part of the default HACS store. If you're not interested in
development branches this is the easiest way to install.  See
[hass-aarlo-hacs](hacsinstall.md) for some hints on installing and setup using
HACS and the home assistant interface.

<a name="installation-from-script"></a>
#### From Script

```sh
install /config
# check output looks good
install go /config
```

Add the following to the top of your UI configuration file.

```yaml
resources:
  - type: module
    url: /local/aarlo-glance.js
```

The card type is `aarlo-glance.js`.


<a name="configuration"></a>
## Configuration

The card supports the following configuration items:

| Name             | Type          | Default        | Supported options                                                                             | Description                                                                                                          |
| -------------    | ------------- | -------------- | ---------------------------------------------------------------------------------------       | -----------------------------------------                                                                            |
| type             | string        | **required**   | `custom:aarlo-glance`                                                                         |                                                                                                                      |
| entity           | string        | **required**   | full camera entity_id                                                                         |                                                                                                                      |
| camera           | string        | **required**   | camera name                                                                                   |                                                                                                                      |
| name             | string        |                | Display Name                                                                                  |                                                                                                                      |
| show             | string list   | **required**   | [motion, sound, snapshot, battery_level, signal_strength, captured_today, image_date, on_off] | all items are optional but you must provide at least 1                                                               |
| hide             | string list   |                | [title, status, date ]                                                                        | Hide this information from the card.                                                                                 |
| top_title        | boolean       | false          |                                                                                               | Show the title at the top of the card                                                                                |
| top_status       | boolean       | false          |                                                                                               | Show the status at the top of the card                                                                               |
| top_date         | boolean       | false          |                                                                                               | Show the date at the top of the card                                                                                 |
| image_view       | string        |                | ['active']                                                                                    | 'active' will change a multicamera card to the latest camera on activity.                                            |
| image_click      | string        | play           | ['modal','smart','stream','recording']                                                        | Action to perform when image is clicked. Remove attribute to play last recorded video when image is clicked.         |
| aspect_ratio     | string        |                | ['square']                                                                                    | Use 'square' if you have a video doorbell camera.                                                                    |
| library_view     | string        |                | ['blended']                                                                                   | 'blended' will join all the camera recordings into a single library.                                                 |
| library_click    | string        |                | ['modal','smart']                                                                             | Action to perform when library image is clicked. Remove attribute to play last recorded video when image is clicked. |
| library_sizes    | integer list  | [3]            |                                                                                               | List of grid sizes for the library to use.                                                                           |
| library_regions  | integer list  | library_sizes  |                                                                                               | List of grid sizes to show trigger item in colored box.                                                              |
| library_animal   | css color     | orangered      | Any valid CSS color                                                                           | Color box to use when an animal triggered the recording.                                                             |
| library_vehicle  | css color     | yellow         | Any valid CSS color                                                                           | Color box to use when an vehicle triggered the recording.                                                            |
| library_person   | css color     | lime           | Any valid CSS color                                                                           | Color box to use when an person triggered the recording.                                                             |
| modal_multiplier | float         | 0.7            | float from 0.1 -> 1.0                                                                         | How big to make the modal window, as a fraction of the main window size.                                             |
| auto_play        | boolean       | false          |                                                                                               | If true, automatically start stream when card is viewed.                                                             |
| max_recordings   | integer       | 99             | Any positive integer.                                                                         | Limit the numnber of videos downloaded for the library viewer.                                                       |
| lang             | string        | en             | Any valid language code.                                                                      | Use the language `lang` whrn displaying the card.                                                                    |
| swipe_threshold  | integer       | 150            | Any positive integer.                                                                         | How far to move to reigster a left swipe on the library card.                                                        |
| card_size        | integer       | 3              | Any positive integer.                                                                         | Tell the UI how big to make the card.                                                                                |
| logging          | boolean       | false          | true of false                                                                                 | If true card will write out debug to console.                                                                        |
| door             | string        | entity_id      |                                                                                               | Useful if the camera is pointed at a door.                                                                           |
| door_lock        | string        | entity_id      |                                                                                               |                                                                                                                      |
| door_bell        | string        | entity_id      |                                                                                               |                                                                                                                      |
| door_bell_mute   | string        | entity_id      |                                                                                               | Switch to mute the chime of the door.                                                                                |
| door2            | string        | entity_id      |                                                                                               | Useful if the camera is pointed at a door.                                                                           |
| door2_lock       | string        | entity_id      |                                                                                               |                                                                                                                      |
| door2_bell       | string        | entity_id      |                                                                                               |                                                                                                                      |
| door2_bell_mute  | string        | entity_id      |                                                                                               | Switch to mute the chime of the door.                                                                                |
| light            | string        | entity_id      |                                                                                               | Control a light near the camera.                                                                                     |
| light_left       | boolean       | false          |                                                                                               | Place light control on left of card                                                                                  |
| camera_id        | string        |                |                                                                                               | Override the calculated camera device name                                                                           |
| motion_id        | string        |                |                                                                                               | Override the calculated motion device name                                                                           |
| sound_id         | string        |                |                                                                                               | Override the calculated sound device name                                                                            |
| battery_id       | string        |                |                                                                                               | Override the calculated battery device name                                                                          |
| signal_id        | string        |                |                                                                                               | Override the calculated signal device name                                                                           |
| capture_id       | string        |                |                                                                                               | Override the calculated captured today device name                                                                   |
| last_id          | string        |                |                                                                                               | Override the calculated last captured device name                                                                    |

### `entity` vs `camera`
You only need one of these. `entity` is the full camera entity id - for
example, `camera.aarlo_front_door` while `camera` is just the name portion -
for example, `aarlo_front_door`.

### Naming
If you don't change entity names you don't need to worry about this.

The code tries to be smart about mapping cameras to device names and as long as
you follow certain rules the card can automatically generate device names.

Given the entity id `camera.aarlo_front_door`, you get a `prefix` of "aarlo_"
and a `camera_name` of "front_door".

Given the entity id `camera.front_door`, you get a `prefix` of "" and a
`camera_name` of "front_door".

The device names are calculated as:
* motion device = `binary_sensor.${prefix}motion_${camera_name}`
* sound device = `binary_sensor.${prefix}sound_${camera_name}`
* battery device = `sensor.${prefix}battery_level_${camera_name}`
* signal_device = `sensor.${prefix}signal_strength_${camera_name}`
* capture_today_device = `sensor.${prefix}captured_today_${camera_name}`
* last_capture_device = `sensor.${prefix}last_${camera_name}`

If you rename any of the `aarlo` using a different scheme you will need to
provide the full device name to get motion, sound, battery, signal, capture or
last capture notifications to work. See the corresponding `*_id` configuration
item.

### `show` options
* `motion`: an icon that indicates when motion is detected
* `sound`: an icon that indicates when sound is detected
* `battery_level`: an icon that shows current battery level
* `signal_strength`: an icon that shows wifi signal strength
* `snapshot`: an icon that takes a snapshot when pressed
* `captured_today`: an icon with descriptive text showing how many recordings
  have been captured today.
* `image_date`: an icon with descriptive text showing when the last image was
  taken
* `on_off`: a switch allowing the camera to be turned off and on

### `image_click` and `library_click`
These are comma separated lists that describe how to handle a click on the
image or library window.
* `stream`: play the live stream, does nothing on library click
* `recording`: play the last recording, does nothing on library click which
  always plays the selected recording
* `modal`: force a modal window, might not work well on mobile devices
* `smart`: use a `modal` window on desktops and `non-modal` on mobile devices.

#### Notes
To get the first four `show` items to work correctly you need to enable the
corresponding `binary_sensor` or `sensor`. For example, to get motion
notifications working you need the following binary sensor enabled:

```yaml
binary_sensor:
  - platform: aarlo
    monitored_conditions:
    - motion
```



#### Example

The following is my front door camera configuration.

```yaml
type: 'custom:aarlo-glance'
entity: camera.aarlo_front_door_camera
name: Front Door
show:
  - motion
  - sound
  - snapshot
  - battery_level
  - signal_strength
  - captured_today
  - image_date
top_title: false
top_status: false
top_date: false
image_click: play
door: binary_switch.front_door
door_lock: lock.front_door_lock
door_bell: binary_switch.aarlo_ding_front_door_bell
door2: binary_switch.front_door
door2_lock: lock.front_door_lock
door2_bell: binary_switch.aarlo_ding_front_door_bell
light: light.aarlo_front_light
```

You don't need to reboot to see the GUI changes, a reload is sufficient. And if
all goes will see a card that looks like this:


<a name="how-it-looks"></a>
## How it looks

![The Image View](/images/arlo-glance-01.png?raw=true)

Reading from left to right you have the camera name, motion detection indicator,
captured clip indicator, battery levels, signal level and current state. If you
click the image the last captured clip will play, if you click the last captured
icon you will be show the video library thumbnails - see below. Clicking the
camera icon (not shown) will take a snapshot and replace the current thumbnail.
(See supported features for list of camera statuses)

Clicking on the last captured clip will display thumbnail mode. Clicking on a
thumbnail starts the appropiate video.  You can currently only see the last 99
videos. If you move your mouse over a thumbnail it will show you time of capture
and, if you have a Smart subscription, a reason for the capture. **>** takes you
to the next page, **<** to the previous and **X** closes the window.

![The Library View](/images/arlo-glance-02.png)


<a name="further-documentation"></a>
##  Further Documentation
See [hass-aarlo](https://github.com/twrecked/hass-aarlo/blob/master/README.md)
for general Aarlo documentation.

