# ha-alexa-tts
Alexa Unsolicted TTS for Home Assistant 

Here is the unsolicited text to speech for Alexa devices that we have been waiting for!

The [loetzimmer.de](https://loetzimmer.de/patches/alexa_remote_control.sh) shell script has been updated (Thanks Ralf Otto) with the excellent work of Michael Geramb to support direct TTS to any Alexa devices!

This script does work by using the Alexa web interface, so it may break at anytime. However, the TTS component is POSTing a JSON object direct to the API endpoint, so the odds that TTS portion stays working are high.

The TTS works with two shells scripts, the original by Alex Loetzimmer and wrapper for it that allows Home Assistant to call it as a [command line notify component](https://www.home-assistant.io/components/notify.command_line/). 

To get started, download the scripts:
https://github.com/walthowd/ha-alexa-tts

Save scripts in your Home Assistant config directory and make them executable (**chmod +x alexa_remote_control.sh & chmod +x alexa_wrapper.sh**)

Edit the **alexa_remote_control.sh** with your login credentials (EMAIL and PASSWORD line). 

Run the script interactively to pull your devices:

    ./alexa_remote_control.sh -a 

 If login fails look at **/tmp/.alexa.login**. Search for "password" and see if you are being prompted for the captcha. If so, you can attempt to login to alexa.amazon.com manually from a browser (from the same IP) and see if that fixes the issue for you. It never did for me, so I logged in to alexa.amazon.com with Chome and used the cookies.txt extension to export my amazon cookies to /tmp/.alexa.cookie.

Once you have **./alexa_remote_control.sh -a** returning your devices you can test TTS by running:

    ./alexa_remote_control.sh -d "My Dot Name" -e speak:This_is_a_test!

If that works, next in a shell type:

    echo $PATH

 Copy that value and put it in the PATH in **alexa_wrapper.sh**

Add a command line notify to Home Assistant:

    notify:
      - platform: command_line
        name: 'My Dot Name'
        command: "/home/homeassistant/.homeassistant/alexa_wrapper.sh -d 'My Dot Name'"
        
 If you are using hass.io, adjust the command line to:
 
         command: ""/config/alexa_wrapper.sh -d 'My Dot Name'"

Restart Home Assistant and you should now be able to call notify.my_dot_name with a message from an automation or the services interface broadcast messages to your devices. 

You can also add a notify for all of your Alexa devices:

    notify:
      - platform: command_line
        name: 'All Alexas'
        command: "/home/homeassistant/.homeassistant/alexa_wrapper.sh -d 'ALL'"

Let me know if you have any questions!
