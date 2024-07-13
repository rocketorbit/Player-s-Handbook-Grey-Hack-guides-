# Universal guide to home securing in v0.8.5 public(The metas change with every update)

**Home securing requires you to delete your current computer and start over, perform the following steps without any mistakes to maximize its effect. If you do not start over, your system may have already been backdoored and no securing can help. If you make any mistakes during the steps, you should also start over as it may also leave a backdoor. Follow the steps strictly and do not do any unnecessary operations, and you will be fine.**

## Things we need:
### This guide
### A text file on your real life computer, hereafter referred to the text file (You should keep any data outside the game)

1. Start the game and begin with singleplayer. Finish tutorials and the first mission in it so that when you join multiplayer for the first time, you have gift.txt and hackshop ip instantly.
2. Compile the following code as `passgen`, and run `passgen 15` to generate a 15 digit alphanumeric password. Copy the password to the text file.
```
if not params then exit("Usage: " + program_path.split("/")[-1] + " [length]")
length = to_int(params[0])
if typeof(length) != "number" or length <= 0 then exit("Length must be a positive integer.")
pass = ""
while length > 0
    pass = pass + "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890"[floor(rnd * 62)]
    length = length - 1
end while
print(pass)
```
3. Reboot to menu. Select delete computer and join multiplayer. This wipes your current multiplayer system.
4. Set the username, os name to what you want, set the password to the one you saved in the text file.
5. After you see the desktop, open Terminal.exe and run the following commands
```
sudo -s
chmod -R o-rwx /
```
6. Repeat step 2, compile the code in multiplayer as `passgen` as well. You will use this script to generate any password you set. For now run `passgen 15` twice to generate two password for mail and bank, save both to the text file.
7. Hack another wifi, save its name and password to the text file.
8. You need to do this step quickly, preferably within a minute to reduce the chance of being found. Connect to the wifi in the gift.txt, open Chat.exe and set a username, Be very careful because you can only set a username once each server wipe(Usually happen about once a year). Open Browser.exe and search for mail, register with a name you want and a password on the text file. After mail, search for bank, register with the other password on the text file. Connect to the wifi on the text file.
9. After you finished step 8, you have used the gift.txt wifi to register your mail and bank, and switched to another wifi. From now on you will not be using the wifi on the gift.txt again. Now compile the following code as `newHackshop`, run it and wait till it gives you a hackshop ip. You do not use the hackshop in your mail because that is permanent and you do not want to lose it. This script gives you a temporary hackshop. You should be very careful not to leak the hackshop ip in the mail because a player only have one permanent hackshop each reset.
```
randomIp = function()
    while true
        ip = floor((rnd * 255) + 1) + "." + floor((rnd * 255) + 1) + "." + floor((rnd * 255) + 1) + "." + floor((rnd * 255) + 1)
        if not is_valid_ip(ip) then continue
        if is_lan_ip(ip) then continue
        return ip
    end while
end function

getRouter = function(ip)
	router = get_router(ip)
	if not router then router = get_switch(ip)
	if not router then return null
	return router
end function

hasRepoService = function(router)
    for lanIp in router.devices_lan_ip
        ports = router.device_ports(lanIp)
        for port in ports
            if router.port_info(port).split(" ")[0] == "repository" then return true
        end for
    end for
    return null
end function

main = function()
    while true
        ip = randomIp
        router = getRouter(ip)
        if not router then continue
        if not hasRepoService(router) then continue
        exit(ip)
    end while
end function
main
```
10. Open Browser.exe, go to the hackshop you got from the script last step, and click on "credentials needed" missions, you want the one says "particular user", accept 8 of them.
11. Use whois and Mail.exe social engineering to complete all of them.
12. Again, you need to do this step quickly, preferably within a minute to reduce the chance of being found. After you finished all 8 missions, you should have enough money to pay for your own wifi. Now go to Browser.exe and search internet, go to a ISP and download ConfigLan.exe, after download buy the cheapest home network. For maximum security, do not set custom domain. After you have ConfigLan.exe downloaded and the home network purchased, open Mail.exe and connect to the wifi it sends you.
13. 
