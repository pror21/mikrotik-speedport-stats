# Mikrotik Speedport Stats

A Mikrotik script that fetch and parses DSL line and device stats from the Speedport Plus modem/router.

Usage:
```
# Load script
/system script run "speedport-stats"; global speedportStats;

# Use data directly
:put ($speedportStats->"dsl_status")

```
Example:

	{
	    /system script run "speedport-stats"; global speedportStats;

	    :local dslStatus
	    :local dslDownstream
	    :local dslDownstreamBits
	    :local dslUpstream
	    :local dslUpstreamBits

	    :set dslStatus ($speedportStats->"dsl_status")
	    :set dslDownstream ($speedportStats->"dsl_downstream")
	    :set dslUpstream ($speedportStats->"dsl_upstream")

	    :tonum dslDownstream
	    :tonum dslUpstream

	    :put ($dslStatus,$dslDownstream,$dslUpstream)
	    
	    # set queue limits
	    :if ($dslStatus = "online" and $dslDownstream > 0 and $dslUpstream > 0) do={
	        :set dslDownstreamBits ($dslDownstream * 1000)
	        :set dslUpstreamBits ($dslUpstream * 1000)

	        /queue tree set [find name="WAN Down"] max-limit=$dslDownstreamBits
	        /queue tree set [find name="WAN Upload"] max-limit=$dslUpstreamBits
	    }
	}


```
# Available data

// http://192.168.1.1/data/Status.json

[
  {
    "vartype": "value",
    "varid": "device_name",
    "varvalue": "Speedport Plus"
  },
  {
    "vartype": "value",
    "varid": "rebooting",
    "varvalue": "0"
  },
  {
    "vartype": "value",
    "varid": "router_state",
    "varvalue": "OK"
  },
  {
    "vartype": "value",
    "varid": "provis_inet",
    "varvalue": "xx3"
  },
  {
    "vartype": "value",
    "varid": "provis_voip",
    "varvalue": "xx0"
  },
  {
    "vartype": "value",
    "varid": "save_fails",
    "varvalue": "0"
  },
  {
    "vartype": "page_title",
    "varid": "title",
    "varvalue": "Speedport Plus Configuration Program"
  },
  {
    "vartype": "status",
    "varid": "loginstate",
    "varvalue": "0"
  },
  {
    "vartype": "status",
    "varid": "status",
    "varvalue": "ok"
  },
  {
    "vartype": "value",
    "varid": "datetime",
    "varvalue": "2022-11-07 17:16:28"
  },
  {
    "vartype": "value",
    "varid": "uptime",
    "varvalue": "17 days, 7 hours, 13 minutes, 49 seconds"
  },
  {
    "vartype": "value",
    "varid": "firmware_version",
    "varvalue": "09022001.00.035_OTE10"
  },
  {
    "vartype": "value",
    "varid": "serial_number",
    "varvalue": "XXXXXXXXXXXX"
  },
  {
    "vartype": "value",
    "varid": "dsl_link_status",
    "varvalue": "online"
  },
  {
    "vartype": "value",
    "varid": "dsl_sync_time",
    "varvalue": "2022-10-26 05:11:04"
  },
  {
    "vartype": "value",
    "varid": "dsl_online_time",
    "varvalue": "2022-10-26 05:11:36"
  },
  {
    "vartype": "value",
    "varid": "dsl_status",
    "varvalue": "online"
  },
  {
    "vartype": "value",
    "varid": "dsl_downstream",
    "varvalue": "34894"
  },
  {
    "vartype": "value",
    "varid": "dsl_upstream",
    "varvalue": "5495"
  },
  {
    "vartype": "value",
    "varid": "dsl_max_downstream",
    "varvalue": "67128"
  },
  {
    "vartype": "value",
    "varid": "dsl_max_upstream",
    "varvalue": "12039"
  },
  {
    "vartype": "value",
    "varid": "dsl_transmission_mode",
    "varvalue": "VDSL2-17A  Annex B"
  },
  {
    "vartype": "value",
    "varid": "dsl_crc_errors",
    "varvalue": "876654"
  },
  {
    "vartype": "value",
    "varid": "dsl_fec_errors",
    "varvalue": "630125"
  },
  {
    "vartype": "value",
    "varid": "dsl_snr",
    "varvalue": "16.8 / 14.6"
  },
  {
    "vartype": "value",
    "varid": "dsl_atnu",
    "varvalue": "52.0 / 34.0"
  },
  {
    "vartype": "value",
    "varid": "vdsl_atnu",
    "varvalue": "34.0"
  },
  {
    "vartype": "value",
    "varid": "vdsl_atnd",
    "varvalue": "52.0"
  },
  {
    "vartype": "value",
    "varid": "wlan_ssid",
    "varvalue": "COSMOTE-XXXXXX"
  },
  {
    "vartype": "value",
    "varid": "wlan_5ghz_ssid",
    "varvalue": "COSMOTE-XXXXXX"
  },
  {
    "vartype": "option",
    "varid": "use_wlan",
    "varvalue": "0"
  },
  {
    "vartype": "option",
    "varid": "use_wlan_5ghz",
    "varvalue": "0"
  },
  {
    "vartype": "option",
    "varid": "use_wps",
    "varvalue": "1"
  }
]

```
## Libraries

### [mikrotik-json-parser](https://github.com/Winand/mikrotik-json-parser)