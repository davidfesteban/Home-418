# Home-418 or How not to die trying to build your smart home or home server. (+ Minecraft at the end)
## Available method for all devices including amazon-available brands like Teckin or any Tuya based device

Disclaimer: This is not a deep guide and I won't provide explanations of for example why I have used a Athlon 3000G. It is just the path I have built based on try'nd error.
I don't want to invest time making a cool breaking site for this, it is why maybe it is too messy.
I have invested nearly 100h reading and playing. So, this is the simplest way to go

Contact on: 
1. [Primary method and prefered one](https://es.linkedin.com/in/david-fern%C3%A1ndez-esteban-b749a9a8) 
2. [Secondary method and probably going to spam folder](mailto:davidfesteban@hotmail.com)

## Brief schema about IoT devices arch.

Wifi Device (Light, socket, whatever) -> Based on ESP32 chips -> Connected to 2,4 GHz wifi -> Connected to Manufacturer Cloud. (Mainly TUYA)

APP -> Send command to Cloud -> Cloud send command to your Device. The communication way can vary because old devices communicate with the cloud by IRC Chat, that is why they don't care about dynamic IP.

Alexa -> translate voice to command routed by the provider skill -> The skill written in Amazon lambda will make a request to the Manufacturer Cloud.


## Building a server

Stay away from Raspberry Pi, Rock Pi or similars. You are not interested in any SBCs. For real.

Right now, I have a 350 € NASA rocket computer. It has a amd A320 chipset (you can use b450 but you dont need it) and Athlon 3000G. 16GB of cheapest RAM trying to look for maximun MHz supported by A320 and 3000G, and cheapest power supply (don't go too cheap) + 250 GB of SSD/M2.

Then if you want to use it aswell as a NAS in mirror stage (data from one HDD is copied to the other one having a duplicity of 50% of the data), maybe the investment is higher. Another 350 € aprox for 2 HDDs of 4.5TB each. (Don't go cheap with the disks. You will regret it!)

This guide is not going to cover how to build a NAS with Ubuntu Server but, if you want to know more, read about ZFS, ZFPOOL and Samba. (Tip: FTP is meh nowadays)

**Be careful, for updating your BIOS for using your CPU, you may need a GPU. You can ask for borrow it in your local computer repair shop**

## Base OS

Ubuntu Server. That's it. There is no better option available.

## Router

For providing security, you want your VPN server to RUN outside your server. I recommend you to buy RT-AX58U. It has WIFI 6 and OpenVPN Server capability available for your entire local network without the need of a client. Asus routers provide you a really good DDNS service. So it is a WIN-WIN. Bye bye dynamic IP issues. (PPTP VPN is okeish but having OpenVPN is the heaven)

**Advice: RUN ALWAYS, from any product that has "Gaming" in its name or even in its description.** 

Buying a Router or any other hardware can be tricky. In the router example there is no a single youtuber even LinusTechTips that really matters about the actual performance of the router. You have to go to the technical specs and compare one by one.

## Docker & Monitoring

Use docker, always. Don't install things on your shiny clean Ubuntu Server. You will have then dozens of services running without a real control of each application because each one can run differently. (By python, java, systemctl/systemd...) 

The first step is to bring **Webmin, Portainer & Cockpit**. I only use Cockpit & Portainer on my day-a-day but, you may need Webmin console.

## First Step into IoT

OpenHAB & Home Assistant are not bad. They are ""too complex"" for just providing a front-end with some HTTP/s requests but they are ok. I use Home Assistant because I think it is less over-engineered. They are perfect for starters entering in this world. If you are a developer, maybe an HTTP server with bootstrap is more than enough. Home Assistant or OpenHAB are going to force you to read their wiki. They try to make things easier for people that don't really know what is an API for example. This is a really opinion based review but, I prefer to just go with bash/python scripts rather than writing xml scripts template on Home Assistant.

Choose your weapon but remember to always use a Docker image based. If you get tired, it is just a button or two commands.

And remember: Each youtuber there selling NordVPN shit, is going to sell you Home Assistant Cloud and the idea of a own-ing a server is not depending on third party clouds.

## Second Step - Intro or Issues

Once you have Home Assistant, you have to get used to its dashboard and the way of customizing it. Remember, you don't need to use their cloud at all.

The problem with every Tuya device based like Teckin or any Amazon/Aliexpress available brands (based on Tuya aswell) is that their chinese cloud is a shit. All the tutorials for having a local cloud of Tuya don't care about the 12 month trial period of the cloud. Also, the cloud is a nightmare for setting it up.

You can try to flash the devices, but the last updates blocked the OTA flashing method

You can try to re-solder with a new chip, but it is highly probable that you burn something. Soldering is harder than I though.

Tasmota is a dream, but we need more pre-flashed devices on the market with Tasmota. If you don't know what is Tasmota, just google it. Or google about Tasmota devices.

Also, having IoT devices over WIFI, without a HUB, is going to drive your Router and your network crazy because of tons of reasons. (SU-MIMO for example)
So, you are really interested on buying a router. Dongknowshow website is the only one that explain things without bullshit.

You can always use Zigbee, but come on... Maybe a solution for you if you have like 10-12 devices, is to have a single router as hub for this connected by a single local ip to your main router.

## Third step

Once you have your Home Assistant Docker running, you can create buttons/helpers that are attached to a HTTP request. Home Assistant is just your front-end, there is no logic.

So, as you cannot flash, or change anything on your devices (unless you really love ESP32 chips and you decided to solder path), you need a way to integrate Home Assistant with Alexa. Home Assistant is going to ask you to pay cloud for it. They are just crazy.

### Why Alexa?

Well, Alexa has the skill and can control your devices sending commands to the cloud. If you try to do so directly, I can assure you that is a nightmare. So you use Alexa as a gateway. But as Home Assistant want to make money with you and the Alexa integration (come on, alexa is on your local net with an open API, wtf Home Assistant? There is an integration for local alexa based on an old api but it doesn't work too good). 

So, first integrate Alexa with VoiceMonkey.io api. (unless you want to create your own Alexa skill) The creator is a genius. He made a virtual "door bell". So if you send a request to VoiceMonkey, it will send a "ring ring" to the virtual "door bell" on Alexa. Then you have to attach with the "rutines or automatizations" of Alexa, the virtual door bell with the actions you want to make over the devices.

END.

## SCHEMA:

Alexa use (Skill) as -> Communication gateway

Home Assistant sends on button click an HTTP request to -> VoiceMonkey API that will send a request to -> Virtual Door Bell inside Alexa -> Alexa reads it and perform an action through -> Tuya Skill (or other provider IoT skill device) -> it sends a request to the Tuya Cloud -> Tuya Cloud sends commands to your devices.

In this way:

- You have script control over your devices.
- It is more long-timed mantainable than other paths because you are using long term available products
- At the end you will have a full understanding of the entire environment


## NEXT steps:

- Home Assistant sends on button click -> Publish on MQTT topic for unifing everything
- Having two switches and two wifi thermo-sensors for switching heaters on each room.
- Alexa -> Skill -> My MQTT broker (getting rid of VoiceMonkey, sry m8)
- Using ESP32 open software devices and getting rid of Tuya little by little. 
- Higrometer/Hidrometer + ESP32 + ESP32 battery board -> MQTT
- Start using Grafana or other dashboard instead of Home Assistant


## Extra
You can download a docker image containing all versions available of a Minecraft server. It is just one click. (I recommend you to use full docker-compose.yml version instead of single docker command)
