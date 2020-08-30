# HomeAssistant GE Switch Timer with Double Tap Disable
This is a custom Node Red Flow for setting a timer which will turn off lights after a specified period of time.  Double Tapping will disabled the timer and turn the light on until it is turned off.

![NodeRed GE Timer Flow](/images/GE_Timer_Flow.png)

# How does it work?
 this flow will track each double tap action and turn on the light setting a global array variable to track the double tap.  When a light in the specified group state changes to on if the global variable is not set a timer will be started and turn the light off when complete.

# How To Install
## Dependencies
  * This process uses the openZwave beta for Home Assistant and MQTT.  If you are not running the beta than this will not work.  See [OpenZWave Beta](https://www.home-assistant.io/integrations/ozw/) for more info on installation and configuration.

## Install Process  
  * Copy the JSON from GE_Timer_Flow.nodered and import the Flow into Node Red.
  * Connect the MQTT Node to your MQTT Server.  The Node is labeled (Any Light Double Tap).  
  * Update groups.yaml so group double_tap_switches contains all entities you want this to impact.
  * Add any associations to your GE Switch so it reports to MQTT the double tap events
  
# Configuration
## Adding devices to the function
  The flow will look for a Home Assistant group(double_tap_switches) for devices to exclude from this process.
  
  double_tap_switches:
  name: "Switches for double tap timer"
  entities:
    - switch.garage_light
  
## Adding the node association
  The Zwave device needs a node assoication for Group 3 to the zwave stick which is node 1.  The easiest way I have found to do this is through MQTT Explorer.  Publish the following:

  Topic: OpenZWave/1/command/addassociation/ (If you have multiple OZW instances you will need to update the number in position 2 to point to the correct instance).

  Detail JSON:
  {"node": {nodeID}, "group":3, "target": "1.0"} 
  

