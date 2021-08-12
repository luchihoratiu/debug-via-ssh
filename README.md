# debug-via-ssh
This GitHub Action allows you to connect to a GitHub Actions runner via SSH for interactive debugging using ngrok.
1
## Features
It works with Ubuntu, macOS and Windows runners.

## Quick usage
```yaml
- name: Start SSH session
  uses: luchihoratiu/debug-via-ssh@main
  with:
    NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    SSH_PASS: ${{ secrets.SSH_PASS }}
```

## Settings
### Mandatory
* **NGROK_AUTH_TOKEN** - The authorization token received from ngrok. See FAQ section for more info.
* **SSH_PASS** - The password used for starting a SSH session. For Windows runners, this password must respect some [minimum complexity requirements](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

### Optional
* **NGROK_REGION** - The region where the ngrok client will connect to host its tunnels. Defaults to **us**.
* **NGROK_TIMEOUT** - The max amount of time ngrok will host its tunnel. Defaults to **21500** (value is in seconds). 

## FAQ
#### How to get ngrok auth token?</summary>

1. Go to https://ngrok.com/
2. Hit Sign up in the top right corner
3. Login via GitHub/Google or Sign up for a standalone account
4. From the given dashboard, you can now get your ngrok auth token

#### What regions are avaliable for ngrok?
See https://ngrok.com/docs for latest information.
* us - United States
* eu - Europe
* ap - Asia/Pacific
* au - Australia
* sa - South America
* jp - Japan
* in - India
