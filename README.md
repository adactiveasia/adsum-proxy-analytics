![Node module](https://img.shields.io/badge/node-module-orange.svg?style=flat)
&nbsp;
![Node ~6.0](https://img.shields.io/badge/node-~%205.7-blue.svg?style=flat)
&nbsp;
![Coverage 77.17%](https://img.shields.io/badge/coverage-77.17%25-yellow.svg?style=flat)

# adsum-proxy-analytics

Adsum Analytics proxy module allows you to send back monitoring information to the adsum servers.

##Usage

    const APAN = require("adsum-proxy-analytics");

    const options = new APAN.ProxyOptions(
        {
            site: 240,
            device: 170,
            server: {
                hostname: "127.0.0.1",
                port: 9002,
                route: "/local-analytics"
            },
            analytics: {
                site: 116,
                key: "f4a8be0670524ee957374a44dfe278e9",
                endpoint: "http://analytics.adsum.io/piwik.php"
            },
            storage: {
                folder: "./data",
                flushInterval: 60000
            }
        }
    );

    const proxy = new APAN.Proxy(options);
    proxy.server.start().then(
        ()=> {
            console.log("Success");
    
            proxy.analytics.ping().then(
                (result)=>{
                    console.log(result);
                }
            );
            setTimeout(()=>{
                proxy.analytics.trackLog(1000, "Test proxy").then(
                    (result)=>{
                        console.log(result);
                    }
                );
            }, 5000);
        },
        (e)=> {
            console.log("Failed !");
            console.log(e);
        }
    );

    //Or Just bind to an existing server
    proxy.server.bind([SERVE]);

##Options

 - site: [SITEID]
 - device: [DEVICEID]
 
 - analytics:
  - endpoint: "http://analytics.adsum.io/piwik.php"
  - site: [ANALYTICSID]
  - key: [TOKEN]
  
 - server:
  - route: "/local-analytics"
  - hostname: "127.0.0.1"
  - port: [PORT]
 
 - storage:
  - folder: "./data"
  - flushInterval: 120000
  
##Methods

###ping()
Send ping to adsum server

###trackLog(SEVERITY, MESSAGE)
Send monitoring information to adsum server with severity level
**SEVERITY** _{number}_
 - NOTICE: 2000
 - WARN:
 - ERR:

**MESSAGE** _{string}_
