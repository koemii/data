# Bitburner - Dashboard Scipts

> These scripts are for the bitburner idle game. You can find it [here.](https://danielyxie.github.io/bitburner/)

**Note:** I'm not the best programmer so these scripts can maybe look akward to you :P

## How to use

- First of all the `setupinfos.script`. It will copy the `hostUpdate.script` from your home computer to each server you have access to. (and obviously it needs some RAM) 

- Each server will get the `hostUpdate.script`. With this script the **server will use 3.60gb**. Slightly expensive, I know. It will generate a txt-file with his own server datas each minute.

- You can use the `serverinfos.script` from your home computer. By using it a little dashboard / table will be generated on your terminal to give you a better overview.

### ATTENTION

- You will have to check the `file`-variables so the script names and folders will be like yours.
- By using all of this, you have to consider the lesser free amount of RAM on each server
- Servers with 0 RAM will be not shown at the moment. Which is not good, I will try to figure something out for that.
- If you like you can change the 60-second sleeping time at the end of the `hostUpdate.script`. Its at the very end.

---

### setupinfos.script

```javascript
var servers = [
    "foodnstuff", "joesguns", "sigma-cosmetics", "hong-fang-tea", "harakiri-sushi", "iron-gym",
    "max-hardware", "nectar-net", "zer0",
    "omega-net", "neo-net", "phantasy", "silver-helix",
    "netlink", "the-hub", "comptek", "avmnite-02h", "crush-fitness", "johnson-ortho",
    "rothman-uni", "syscore", "catalyst", "I.I.I.I", "zb-institute", "summit-uni",
    "rho-construction", "lexo-corp", "millenium-fitness", "aevum-police", "alpha-ent",
    "aerocorp", "unitalife", "snap-fitness", "taiyang-digital", "solaris", "omnia",
    "galactic-cyber", "nova-med", "zeus-med", "zb-def", "infocomm",
    "univ-energy", "icarus", "defcomm", "deltaone", "global-pharm"
];

file = "/dash/hostUpdate.script";

var i;
for (i = 0; i < servers.length; i++) {
    server = servers[i];

    if (hasRootAccess(server)) {
        ram = getServerRam(server);
        if (ram[0] > 0) {
            scp(file, server);
            sleep(1000);
            exec(file, server, 1);
        }
    }
}
tprint("finished");
```

---

### serverinfos.script

```javascript
var servers = [
    "foodnstuff", "joesguns", "sigma-cosmetics", "hong-fang-tea", "harakiri-sushi", "iron-gym",
    "max-hardware", "nectar-net", "zer0",
    "omega-net", "neo-net", "phantasy", "silver-helix",
    "netlink", "the-hub", "comptek", "avmnite-02h", "crush-fitness", "johnson-ortho",
    "rothman-uni", "syscore", "catalyst", "I.I.I.I", "zb-institute", "summit-uni",
    "rho-construction", "lexo-corp", "millenium-fitness", "aevum-police", "alpha-ent",
    "aerocorp", "unitalife", "snap-fitness", "taiyang-digital", "solaris", "omnia",
    "galactic-cyber", "nova-med", "zeus-med", "zb-def", "infocomm",
    "univ-energy", "icarus", "defcomm", "deltaone", "global-pharm"
];

var today = new Date();
tprint("<font color=red>|----------------------------------------[" + today.toLocaleTimeString() + "]------------------------------------------------|</font>");
tprint("<font color=gray>ram   server name  \t\t  cur | max€\tcSec | mSec\t     grow | [rHL] P</font>");

var i;
for(i = 0; i < servers.length; i++){
    server = servers[i];

    if(serverExists(server)){
        output = "";
        file = server + ".txt";
        if(fileExists(file, server)){
            scp(file, server, "home");
            output += read(file) + "\n";
            rm(file);
            tprint(output);
        }
    } 
}

var today = new Date();
tprint("|-----------------------------------------]" + today.toLocaleTimeString() + "[--------------------------------------------|");
```

---

### hostUpdate.script

```javascript
hostname = getHostname();

while (true)
{

    output = "";
    ram = getServerRam(hostname);

    newHostname = hostname;
    if (hostname.length > 16) {
        newHostname = hostname.substring(0, 16);
    } else if (hostname.length < 16) {
        toLess = 16 - hostname.length;
        for (x = 0; x < toLess; x++) {
            newHostname += " ";
        }
    }

    // MONEY
    curMoney = getServerMoneyAvailable(hostname) / 1000000;
    curMoneyLen = 0;
    if (curMoney >= 10000)
    {
        curMoney = nFormat(curMoney, "0") / 10000;
        curMoney = nFormat(curMoney, "0.0");
        curMoneyLen = curMoney.length;
        curMoney = "<font color=DarkSlateBlue>" + curMoney + "b</font>";
    } else if (curMoney >= 1000)
    {
        curMoney = nFormat(curMoney, "0") / 1000;
        curMoney = nFormat(curMoney, "0.0");
        curMoneyLen = curMoney.length;
        curMoney = "<font color=DarkSlateBlue>" + curMoney + "b</font>";
    } else
    {
        curMoneyLen = curMoney.length;
        curMoney = "<font color=DarkSlateGray>" + nFormat(curMoney, "0") + "m</font>";
    }

    maxMoney = getServerMaxMoney(hostname) / 1000000;
    maxMoneyLen = 0;
    if (maxMoney >= 10000)
    {
        maxMoney = nFormat(maxMoney, "0") / 10000;
        maxMoney = nFormat(maxMoney, "0.0");
        maxMoneyLen = maxMoney.length;
        maxMoney = "<font color=DarkSlateBlue>" + maxMoney + "b</font>";
    } else if (maxMoney >= 1000)
    {
        maxMoney = nFormat(maxMoney, "0") / 1000;
        maxMoney = nFormat(maxMoney, "0.0");
        maxMoneyLen = maxMoney.length;
        maxMoney = "<font color=DarkSlateBlue>" + maxMoney + "b</font>";
    } else
    {
        maxMoneyLen = maxMoney.length;
        maxMoney = "<font color=DarkSlateGray>" + nFormat(maxMoney, "0") + "m</font>";
    }
    subCur = curMoney.substring(26, curMoney.length - 8);
    curMoneyLen = subCur.length;
    var a;
    var ma = 4 - curMoneyLen;
    for (a = 0; a < ma; a++)
    {
        curMoney = " " + curMoney;
    }
    maxCur = maxMoney.substring(26, maxMoney.length - 8);
    maxMoneyLen = maxCur.length;
    var ma = 4 - maxMoneyLen;
    for (a = 0; a < ma; a++)
    {
        maxMoney = " " + maxMoney;
    }
    money = curMoney + "<font color=gray> | </font>" + maxMoney;


    // SECURITY
    curSec = getServerSecurityLevel(hostname);
    curSecurity = nFormat(curSec, "0");
    minSec = getServerMinSecurityLevel(hostname);
    minSecurity = nFormat(minSec, "0");
    security = "<font color=lime>" + curSecurity + "</font><font color=gray> | </font><font color=lime>" + minSecurity + "</font>";

    grow = getServerGrowth(hostname);
    rhl = getServerRequiredHackingLevel(hostname);
    ports = getServerNumPortsRequired(hostname);

    output = "<font color=orange>" + ram[0] + "</font>" + "\t" +
        "<font color=white>" + newHostname + "</font>" + "\t" +
        money + "\t   " + 
        security + 
        "<font color=gray>\t\t</font>" +grow + 
        "<font color=gray> | </font>" +
        "<font color=orange>[" + rhl + "]</font>"+
        "<font color=white> "+ ports +"</font>";


    write(hostname + ".txt", output, "w");
    sleep(60000);
}
```