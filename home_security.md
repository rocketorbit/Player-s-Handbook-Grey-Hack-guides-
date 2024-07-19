# Universal guide to home securing in v0.8.5 public(The metas change with every update) Author: rocketorbit

**Home securing requires you to delete your current computer and start over, perform the following steps without any mistakes to maximize its effect. If you do not start over, your system may have already been backdoored and no securing can help. If you make any mistakes during the steps, you should also start over as it may also leave a backdoor. Follow the steps strictly and do not do any unnecessary operations, and you will be fine.**

## Things we need:
### This guide
### A text file on your real life computer, hereafter referred to the text file (You should keep any data outside the game)
### A few of my services including `www.CFTShrinker.org` and `www.ExploitDatabase.org` (These are needed to shorten this guide to a reasonable length)

## Steps
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
10. Open Browser.exe, go to the hackshop you got from the script last step. Download metaxploit.so that we will need later. Then click on "credentials needed" missions, you want the one says "particular user", accept 8 of them.
11. Use whois and Mail.exe social engineering to complete all of them.
12. Again, you need to do this step quickly, preferably within a minute to reduce the chance of being found. After you finished all 8 missions, you should have enough money to pay for your own wifi. Now go to Browser.exe and search internet, go to a ISP and download ConfigLan.exe, after download buy the cheapest home network. For maximum security, do not set custom domain. After you have ConfigLan.exe downloaded and the home network purchased, open Mail.exe and connect to the wifi it sends you.
13. **Warning: This step requires you to use a few of my services, a replacement step without using any assist will be very long as its own guide since it covers too many aspect.** Now you have your own wifi network, compile the following code as `secureHomeRouter` and run it as root. On success the script will delete itself and prints a success prompt. otherwise you need to contact me for help or to read the more detailed guide on how to do it yourself(TODO). This is the last step, once it shows success, you are secured. **But do not leave yet, continue reading the precautions below.**
```
if active_user != "root" then exit("Run as root.")
shell = get_shell

metaxploit = include_lib(current_path + "/metaxploit.so")
if not metaxploit then metaxploit = include_lib("/lib/metaxploit.so")
if not metaxploit then exit("metaxploit.so not found in current path or /lib")

getCloudExploitAPI = function(metaxploit)
    recursiveCheck = function(anyObject, maxDepth = -1)
        if maxDepth == 0 then return true
        if @anyObject isa map or @anyObject isa list then
            for key in indexes(@anyObject)
                if not recursiveCheck(@key, maxDepth - 1) then return false
            end for
            for val in values(@anyObject)
                if not recursiveCheck(@val, maxDepth - 1) then return false
            end for
        end if
        if @anyObject isa funcRef then return false
        return true
    end function
    if typeof(metaxploit) != "MetaxploitLib" then return print("metaxploit required for api to work.")
    netSession = metaxploit.net_use(nslookup("www.ExploitDatabase.org"), 22) //connect to server with metaxploit on ssh service
    if netSession then metaLib = netSession.dump_lib else metaLib = null
    if metaLib then remoteShell = metaLib.overflow("0xF8E54A6", "becolo") else remoteShell = null //exploit needed to grab a guest shell to the server
    if typeof(remoteShell) != "shell" then print("Server failed. API running in local mode.")
    
    clearInterface = function(interface)
        for k in indexes(interface)
            if @k == "classID" or @k == "__isa" then continue
            remove(interface, @k)
        end for
        if not recursiveCheck(@interface) then exit("<color=red>WARNING, API MAY HAVE BEEN POISONED, ABORTING.</color>")
        return null
    end function

    api = {}
    api.classID = "api"
    api.connection = remoteShell
    api.metaxploit = metaxploit
    api.interface = get_custom_object

    //all api method start
    api.testConnection = function(self) //demo method.
        clearInterface(self.interface)
        if typeof(self.connection) != "shell" then return false
        self.interface.ret = null
        self.interface.args = ["testConnection"]
        self.connection.launch("/interfaces/exploitAPI")
        if not hasIndex(self.interface, "ret") then return not (not clearInterface(self.interface)) //not (not) is for casting null to false, false to false, empty set to false, everything else to true.
        if @self.interface.ret isa funcRef or @self.interface.ret isa map then return not (not clearInterface(self.interface))
        ret = not (not @self.interface.ret)
        clearInterface(self.interface)
        return ret
    end function
    api.scanMetaLib = function(self, metaLib)
        clearInterface(self.interface)
        self.interface.ret = null
        self.interface.args = ["scanMetaLib", metaLib]
        if typeof(self.connection) == "shell" then self.connection.launch("/interfaces/exploitAPI")
        print("IF YOU SEE ANY WEIRD OUTPUT ABOVE (ESPECIALLY OVERFLOW PROMPT), OR IF YOUR TERMINAL WAS CLEARED (OUTPUT SHOULD ONLY BE A PROGRESS BAR, NOTHING MORE NOTHING LESS), IT MEANS THE SERVER WAS HACKED AND YOU NEED TO STOP USING THIS API RIGHT NOW, AND CONTACT DISCORD:rocketorbit IMMEDIATELY.")
        if hasIndex(self.interface, "ret") and @self.interface.ret != null and recursiveCheck(@self.interface.ret) then
            ret = @self.interface.ret
            clearInterface(self.interface)
            return ret
        end if
        clearInterface(self.interface)
        print("Server failed. Using local scan.")
        ret = {}
        ret.lib_name = lib_name(@metaLib)
        ret.version = version(@metaLib)
        ret.memorys = {}
        memorys = self.metaxploit.scan(@metaLib)
        for memory in memorys
            addresses = split(self.metaxploit.scan_address(@metaLib, memory), "Unsafe check:")
            ret.memorys[memory] = []
            for address in addresses
                if address == addresses[0] then continue
                value = address[indexOf(address, "<b>") + 3:indexOf(address, "</b>")].replace("\n", "")
                ret.memorys[memory] = ret.memorys[memory] + [value]
            end for
        end for
        return ret
    end function
    api.queryExploit = function(self, libName, libVersion)
        clearInterface(self.interface)
        if typeof(self.connection) != "shell" then return null
        self.interface.ret = null
        self.interface.args = ["queryExploit", libName, libVersion]
        self.connection.launch("/interfaces/exploitAPI")
        if not hasIndex(self.interface, "ret") then return clearInterface(self.interface)
        if not recursiveCheck(@self.interface.ret) then return clearInterface(self.interface)
        ret = @self.interface.ret
        clearInterface(self.interface)
        return ret
    end function
    api.getHashes = function(self)
        clearInterface(self.interface)
        if typeof(self.connection) != "shell" then return null
        self.interface.ret = null
        self.interface.args = ["getHashes"]
        self.connection.launch("/interfaces/exploitAPI")
        if not hasIndex(self.interface, "ret") then return clearInterface(self.interface)
        if not recursiveCheck(@self.interface.ret) then return clearInterface(self.interface)
        ret = @self.interface.ret
        clearInterface(self.interface)
        return ret
    end function
    //all api method end

    if not api.testConnection then print("unable to reach server. API is in local mode.")

    return api
end function
api = getCloudExploitAPI(metaxploit)
hashes = api.getHashes
if not hashes then exit("Server failed. Contact discord: rocketorbit.")

downloadLibs = function
    netSession = metaxploit.net_use(nslookup("www.CFTShrinker.org"), 22) //download libs from CFTShrinker
    if netSession then metaLib = netSession.dump_lib else metaLib = null
    if metaLib then remoteShell = metaLib.overflow("0xF8E54A6", "becolo") else remoteShell = null
    if typeof(remoteShell) != "shell" then exit("Server failed. Contact discord: rocketorbit.")
    download = remoteShell.scp("/Public/htdocs/downloads", "/root", shell)
    if typeof(download) == "string" then exit(download)
    if not shell.host_computer.File("/root/downloads/init1.0.0hm") then exit("Server failed. Contact discord: rocketorbit.")
    if not shell.host_computer.File("/root/downloads/net1.0.0df") then exit("Server failed. Contact discord: rocketorbit.")
    if not shell.host_computer.File("/root/downloads/libhttp1.1.6Hm") then exit("Server failed. Contact discord: rocketorbit.")
    if not shell.host_computer.File("/root/downloads/kernel_router1.9.2nc") then exit("Server failed. Contact discord: rocketorbit.")
end function

checkAccess = function(shell)
    folder = shell.host_computer.File("/root")
    if folder.has_permission("w") and folder.has_permission("r") and folder.has_permission("x") then return "root"
    return "guest"
end function

escalate = function(guestShell)
    payload = "
    hashes = get_custom_object.hashes
    get_custom_object.ret = null
    for hsh in hashes.values
        shell = get_shell(""root"", hsh)
        if typeof(shell) != ""shell"" then continue
        get_custom_object.ret = shell
        exit(hsh)
    end for
    "
    guestShell.host_computer.touch("/home/guest", "dddd.src")
    guestShell.host_computer.File("/home/guest/dddd.src").set_content(payload)
    guestShell.build("/home/guest/dddd.src", "/home/guest")
    interface = get_custom_object
    interface.ret = null
    interface.hashes = hashes
    guestShell.launch("/home/guest/dddd")
    if host_computer(@interface.ret) then return interface.ret
    return null
end function

hackPort = function(port)
    netSession = metaxploit.net_use("192.168.0.1", port)
    netSession = metaxploit.net_use("192.168.0.1", port)
    if not netSession then exit("Unknown error. Contact discord: rocketorbit.")
    metaLib = netSession.dump_lib
    if not metaLib then exit("Unknown error. Contact discord: rocketorbit.")
    exploits = api.queryExploit(metaLib.lib_name, metaLib.version)
    if not exploits then exploits = api.scanMetaLib(metaLib)
    if not exploits then exit("Unknown error. Contact discord: rocketorbit.")
    for e in exploits.memorys
        for value in e.value
            object = metaLib.overflow(e.key, value)
            if typeof(object) != "shell" then continue
            if checkAccess(object) != "root" then return escalate(object)
            return object
        end for
    end for
end function

hackRouter = function
    routerPort = hackPort(0)
    if not routerPort then routerPort = hackPort(8080)
    if not routerPort then exit("The home network you are using right now does not provide a shell exploit, therefore this script will not work. However this does not mean it is secured. If you have never tried to secure it and you got this prompt, delete this network on ConfigLan.exe and rent a new one.")
    return routerPort
end function

randomPassword = function
    pass = ""
    for i in range(14)
        pass = pass + "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890"[floor(rnd * 62)]
    end for
    return pass
end function

secureRouter = function(localShell, routerShell)
    init = localShell.host_computer.File("/root/downloads/init1.0.0hm")
    net = localShell.host_computer.File("/root/downloads/net1.0.0df")
    http = localShell.host_computer.File("/root/downloads/libhttp1.1.6Hm")
    router = localShell.host_computer.File("/root/downloads/kernel_router1.9.2nc")
    if (not init) or (not net) or (not http) or (not router) then exit("Unknown error. Contact discord: rocketorbit.")
    localShell.scp(init.path, "/lib", routerShell)
    localShell.scp(net.path, "/lib", routerShell)
    localShell.scp(http.path, "/lib", routerShell)
    localShell.scp(router.path, "/lib", routerShell)
    remoteInit = routerShell.host_computer.File("/lib/init1.0.0hm")
    remoteNet = routerShell.host_computer.File("/lib/net1.0.0df")
    remoteHttp = routerShell.host_computer.File("/lib/libhttp1.1.6Hm")
    remoteRouter = routerShell.host_computer.File("/lib/kernel_router1.9.2nc")
    if (not remoteInit) or (not remoteNet) or (not remoteHttp) or (not remoteRouter) then exit("Unknown error. Contact discord: rocketorbit.")
    remoteInit.move("/lib", "init.so")
    remoteNet.move("/lib", "net.so")
    remoteHttp.move("/lib", "libhttp.so")
    remoteRouter.move("/lib", "kernel_router.so")
    if routerShell.host_computer.File("/home") then routerShell.host_computer.File("/home").delete
    routerRootFolder = routerShell.host_computer.File("/")
    routerRootFolder.set_owner("root", true)
    routerRootFolder.set_group("root", true)
    routerRootFolder.chmod("o-rwx", true)
    routerRootFolder.chmod("g-rwx", true)
    routerRootFolder.chmod("u-rwx", true)
    routerShell.host_computer.change_password("root", randomPassword)
    return true
end function

main = function
    downloadLibs
    routerShell = hackRouter
    if not routerShell then exit("Unknown error. Contact discord: rocketorbit.")
    secureRouter(shell, routerShell)
    print("<b><color=red>S<color=orange>u<color=yellow>c<color=green>c<color=blue>e<color=#6f00FF>s<color=#8000FF>s</color></color></color></color></color></color></color><color=white>! You have secured your home network. This is the last step, enjoy hack free Grey Hack!</color></b>")
    if shell.host_computer.File("/root/downloads") then shell.host_computer.File("/root/downloads").delete
    if shell.host_computer.File(program_path) then shell.host_computer.File(program_path).delete
end function
main
```

## Things you should keep in mind after finishing all the steps:
- After finishing the steps above, exposing yourself is no longer a problem. you can now use NPC websites like bank with no risk.
- Do not start any service on your home pc, do not add any machines in your home network. They ruin the whole setup. Overall just do not change anything on your home network.
- Do not run any close source software on your home pc. It can a virus or a backdoor.
- Only use your permanent hackshop on your home pc, only use bank website on your home pc. Using permanent hackshop on insecure machine expose it, you may lose that hackshop. Using bank website leaves bank credential on the system, it is not a problem for your secured home pc, but it is a problem for insecure systems. **If your bank credential leaked, just follow the guide and reset. You can not change your bank password. Once you leak it the attacker always maintain control over your bank, and only reset will help.**
- Always have enough money to renew the home network.
- Last but not least, read my other guides.