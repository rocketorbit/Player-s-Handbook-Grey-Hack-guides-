# An explanation to the home security guide in v0.8.5 public(metas change over update) Author: rocketorbit

**If you have not seen the home security guide, please go read that(home_security.md) instead. This is an explanation for that guide.**

## The main idea of this guide

### Devide the guide into three parts.
The first part is about a fresh start, making sure all your password is secure and no backdoors on your system.

The middle part is about not getting found while you are trying to setup everything correctly.

The last part is about setting up to the maximum, to a point where you do not have to worry about exposing anymore.
### So to summarize, the first part ensures that the later parts work, the middle part ensures that you cannot be found when you can be hacked, and the last part ensures that you cannot be hacked when you can be found.

## A brief introduction to each step

1. This step is here so you skip tutorial and first mission when you join multiplayer.
2. In this step you generate the password and later use that as the OS password in multiplayer. This step is not necessary. In fact OS password(password on your home) is the only password that is not important, and if you do know what you are doing, you can be secured with a password like `p`. However this step is here to make sure new players do not set their OS password to a password they use in real life. If you are confident, skip this step.
3. This step we reset our multiplayer machine. This makes sure that you will not have any backdoor when proceeding to the next steps. This is one of the important steps in the guide, do not skip it unless you are really sure of what you are doing.
4. Setup your OS.
5. Again, this step is not necessary. Here we do a very basic securing by removing all permission for guest(the user with group `other`), this step does not help with security and skipping it does not hurt security. However this step is here with two purpose: First new players make sure they have their OS password saved and are able to find them. Second for certain lib version this step is actually necessary(we are not using such lib in our guide however).
6. This step asks you to generate your bank and mail passwords. This is very important as if someone have your bank or mail credentials they can login from anywhere.
7. Here we prepare a extra wifi for the next step.
8. In this step you connect to a wifi first. After you are connected to the internet, you can be hacked. But at this moment your ip is unknown and people can not find you. Now is when you register your Chat.exe username. After you finish with the username, you will use this wifi on mail and bank register. **As soon as you click into any website you found on search, you are exposed. So yes, you can get hacked by simply registering mail and bank. In fact, this is the most common reason for new players to get hacked. Even old players sometimes get hacked by this when they reset.** Once you click on any website, your ip is inside the log of that website, now anyone controlling . This is why once you register both service you need, you should switch wifi. After you switch wifi your old ip still leaves in the websites, but you are gone. **However it is important to know that you should only use those wifi once, as there is a chance that the moment you connect back to a wifi that was once exposed, you immediately get hacked by an automatic script with complex loops that monitors every exposed ip.**
9. In this step you get a hackshop with the script I provided. A hackshop is not a website listed on search and it spreads all across the ipv4 range, so you wont be exposed by visiting a new one you just generated, at least not more exposed than simply connecting to an wifi for the first time. The permanent hackshop I talked about in the guide is the hackshop given to you in the mail. This is about the despawn mechanics, randomly generated website will despawn after a certain amount of time, but the one binded to player will not despawn.
10. Accept missions that you can do without hacking, 8 of them would be money enough to keep the network alive for more than one game year or 30 real life days. considering most of the time you will be offline and play games better than Grey Hack, this is probably enough for you to last a whole wipe. If you want to earn more money after you are done with the guide and the setup, simply doing one is enough for 2 game months.
11. This steps roughly tells you how to do the mission. If you do not know how, you need to go to singleplayer and finish tutorial and first mission to learn the basics.
12. This steps again asks you to visit a npc website, for the same reason as step 8 it expose you so you need to do this quick. After you rent your own home network you can proceed to the last step.
13. This step is fully automated with my script because it is hard to explain things in the guide and have new players to keep up. Overall this step secures the home network by taking advantage of the exploit system. We will go into this detail later down below.

## Supplements to some details

TODO