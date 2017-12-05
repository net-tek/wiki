<!-- TITLE: Get External Ip Address With Curl -->
<!-- SUBTITLE: A quick summary of Get External Ip Address With Curl -->

# Get external IP address with curl

```sh
curl -s http://ipchicken.com | egrep -o '([[:digit:]]{1,3}\.){3}[[:digit:]]{1,3}'
```
