## Context

For this example you will be generating the the cert packages outlined below and using them in IoTivity for manufacturer certificate based OTM between `provisioningclient` and `sampleserver_mfg`

_client.zip_
* Client side cert material to be used by `provisioningclient`
* For the client UUID we will use 12121212-1212-1212-1212-121212121212

_server.zip_
* Server side cert material to be used by `sampleserver_mfg`
* For the server UUID we will use 89898989-8989-8989-8989-898989898989

Shorthand used in this tutorial
* `${iotivity}`
  - Path to your IoTivity root directory (i.e. the dir created by the IoTivity clone)
* `${provisioning_samples}`
  - Path to the IoTivity security provisioning examples
  - i.e. `{iotivity}`/out/linux/x86_64/debug/resource/csdk/security/provisioning/sample
* `${security_tools}`
  - Path to the IoTivity security tools
  - i.e. `{iotivity}`/out/linux/x86_64/debug/resource/csdk/security/tool

Notes:
* The above paths assume an `x86_64/debug` build, you may need to adjust the platform and build type for your particular case.
* The tutorial below assumes you are running on a linux system running a bash shell
* The certificates that you will be downloading expire in 30 days, so you will need to repeat this process at that point.
* For a detailed description of certificate usage within IoTivity, you can read [this](https://workspace.openconnectivity.org/apps/org/workgroup/security_wg/download.php/13441/OCF%20Certificates%20within%20IoTivity%20(v1).docx).

This example assumes that you are familiar with cloning and building IoTvitiy, as well as running the sample provisioning applications

## Generating the Certificates

At https://testcerts.kyrio.com, select **Get Certificate** in the OCF cert download panel

First Download
* **Manufacturer**: Test Mfg
* **Device Name**: Test Client
* **Subject**: 12121212-1212-1212-1212-121212121212
* **Certificate ID**: client

Second Download
* **Manufacturer**: Test Mfg
* **Device Name**: Test Server
* **Subject**: 89898989-8989-8989-8989-898989898989
* **Certificate ID**: server

There will now be `client.zip` and `server.zip` files in your Download directory. At a terminal command line (assuming linux/bash), do the following:

````bash
$ iotivity=~/iotivity # or where ever your iotivity base dir is located)
$ provisioning_samples=${iotivity}/out/linux/x86_64/debug/resource/csdk/security/provisioning/sample
$ security_tools=${iotivity}/out/linux/x86_64/debug/resource/csdk/security/tool
$ cd ~/Downloads (or whatever directory the certs were saved)
$ cp client.zip server.zip ${provisioning_samples}/.
$ cd ${provisioning_samples}
$ unzip client.zip
$ unzip server.zip
````

This results in the creation of  `client` and `server` server directories, which hold certificates that we will use to configure IoTivity.

## Configuring IoTivity

In the `${provisioning_samples}` directory are the following IoTivity configuration files:
* **oic_svr_db_client.json**: config fole for `provisioningclient` resources
* **oic_svr_db_server_mfg.json**: config fole for `sampleserver_mfg` resources

Using your favorite editor, modify `oic_svr_db_client.json` as follows.  (NOTE: for the sake of brevity, many entries that won't be modified are not shown below.  Just keep in mind that you will only be replacing values of existing entries, you will not be adding or removing any entries).

````json
{
    "credid": 1,
    "subjectuuid": "12121212-1212-1212-1212-121212121212",
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./client/client_cert.der.hex` HERE"
},
    "credusage": "oic.sec.cred.mfgcert",
    "privatedata": {
        "encoding": "oic.sec.encoding.raw",
        "data": "PASTE CONTENTS OF ./client/client_key.der.hex HERE"
    }
},
{
    "credid": 2,
    "subjectuuid": "12121212-1212-1212-1212-121212121212",
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./client/chain/1-subca-cert.der.hex` HERE"
    },
    "credusage": "oic.sec.cred.mfgcert"
},
{
    "credid": 3,
    "subjectuuid": "*",
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./server/chain/0-root-cert.der.hex` HERE",
        "revstat": false
    },
    "credusage": "oic.sec.cred.mfgtrustca"
}
````

Similarly, configure `oic_svr_db_server_mfg.json` as shown below


````json
{
    "credid": 1,
    "subjectuuid": "89898989-8989-8989-8989-898989898989",
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./server/server_cert.der.hex` HERE"
    },
    "credusage": "oic.sec.cred.mfgcert",
    "privatedata": {
        "encoding": "oic.sec.encoding.raw",
        "data": "PASTE CONTENTS OF `./server/server_key.der.hex` HERE"
    }
},
{
    "credid": 2,
    "subjectuuid": "89898989-8989-8989-8989-898989898989",
    "credtype": 8,
    "publicdata": {
            "encoding": "oic.sec.encoding.der",
            "data": "PASTE CONTENTS OF `./server/chain/1-subca-cert.der.hex` HERE"
        },
        "credusage": "oic.sec.cred.mfgcert"
},
{
    "credid": 3,
    "subjectuuid": "*",
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./client/chain/0-root-cert.der.hex` HERE",
        "revstat": false
    },
    "credusage": "oic.sec.cred.mfgtrustca"
}
````

Next, you will convert these two .json files to CBOR encoded .dat files, which are read in by `provisioningclient` and `sampleserver_mfg` at startup for initial configuration

````bash
$ cd ${provisioning_samples}
$ ${security_tools}/json2cbor ./oic_svr_db_client.json ./oic_svr_db_client.dat
$ ${security_tools}/json2cbor ./oic_svr_db_server_mfg.json ./oic_svr_db_server_mfg.dat
````

You now have versions of `oic_svr_db_client.dat` and `oic_svr_db_server_mfg.dat` which hold the certificates that you downloaded.  Now run `provioningclient` and `sampleserver_mfg` as you normally would.  They will read in the .dat files that you just created for initial resource configuration.

In order to go through the OTM process, enter **10** followed by **20** followed by **10** at the `provisioning_client` command prompt.  If all went well you will see the UUID `12121212-1212-1212-1212-121212121212` in the list of devices that have been onboarded.


## Configuring for CTT Testing

For CTT testing of IoTivity, the process will be similar to that described above, except you will only be downloading and configuring the server side credentials.

Download a client certificate package as described above for the server, and then configure `oic_svr_db_server_mfg.json` as shown below


````json
{
    "credid": 1,
    "subjectuuid": "89898989-8989-8989-8989-898989898989", // or whatever UUID you are using
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE CONTENTS OF `./server/server_cert.der.hex` HERE"
    },
    "credusage": "oic.sec.cred.mfgcert",
    "privatedata": {
        "encoding": "oic.sec.encoding.raw",
        "data": "PASTE CONTENTS OF `./server/server_key.der.hex` HERE"
    }
},
{
    "credid": 2,
    "subjectuuid": "89898989-8989-8989-8989-898989898989",
    "credtype": 8,
    "publicdata": {
            "encoding": "oic.sec.encoding.der",
            "data": "PASTE CONTENTS OF `./server/chain/1-subca-cert.der.hex` HERE"
        },
        "credusage": "oic.sec.cred.mfgcert"
},
{
    "credid": 3,
    "subjectuuid": "*",
    "credtype": 8,
    "publicdata": {
        "encoding": "oic.sec.encoding.der",
        "data": "PASTE DER.HEX ENCODED STRING FOR CTT ROOT CERTIFICATE HERE",
        "revstat": false
    },
    "credusage": "oic.sec.cred.mfgtrustca"
}
````

You will insert the CTT Root certificate material into `oic_svr_db_server_mfg.json` as shown above.  Create the .dat file for the server as shown above:

````bash
$ cd ${provisioning_samples}
$ ${security_tools}/json2cbor ./oic_svr_db_server_mfg.json ./oic_svr_db_server_mfg.dat
````

Run `sampleserver_mfg` for cert based CTT tests.  When prompted by the CTT for the server root certificate you will paste the content of `./server/chain/0-root-cert.pem` into the CTT prompt.

