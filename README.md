# debug-via-ssh

This GitHub Action allows you to connect to a GitHub Actions via SSH for interactive debugging using ngrok.

## Security warning

This action is intended only for temporary, user-authorized debugging of your own GitHub Actions jobs.

Do **not** use this action:

* on untrusted pull requests;
* in jobs that have access to production secrets;
* in jobs where exposing the runner shell would create unacceptable risk;
* as a persistence, bypass, command-and-control, or unauthorized-access mechanism.

Anyone with the generated SSH endpoint and the configured SSH password (or the private key corresponding to the configured public key) can access the runner while the tunnel is active. Treat the SSH password and/or private key + the printed connection details as sensitive.

This action uses ngrok to create the SSH tunnel. ngrok's own terms, privacy policy, limits, and security model apply. You are responsible for deciding whether ngrok is appropriate for your workflow.

For security sensitive workflows, prefer pinning this action to a trusted release tag or commit SHA instead of using `@main`.

## Privacy

This action does not intentionally collect, store, or transmit user personal data, repository contents, secrets, runner data, the ngrok auth token, or the SSH password to the maintainer of this repository.

The action downloads and runs ngrok, then configures ngrok with the `NGROK_AUTH_TOKEN` value you provide. ngrok may process tunnel metadata or other data according to ngrok's own terms and privacy policy.

Because this action exposes the GitHub Actions runner over SSH, users are responsible for ensuring that secrets, credentials, private code, personal data, and other sensitive information are not exposed during the debugging session.

## Features

It works with Ubuntu, macOS and Windows runners (x86_64 and ARM).

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
* **SSH_PUBLIC_KEY** (can be omitted, if `SSH_PASS` is specified) - The public key used for starting a SSH session. DO NOT put your private key here - private keys should never leave your computer.
* **SSH_PASS** (can be omitted, if `SSH_PUBLIC_KEY` is specified) - The password used for starting a SSH session. For Windows runners, this password must respect some [minimum complexity requirements](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

### Optional

* **NGROK_REGION** - The region where the ngrok client will connect to host its tunnels. Defaults to **us**.
* **NGROK_TIMEOUT** - The max amount of time ngrok will host its tunnel. Defaults to **21500** (value is in seconds).
* **SSH_COMMAND_EXTRA_ARGS** - Extra arguments to append to the printed ssh command. This will be appended to the command exactly as is.

## How it works

The action prepares the runner for SSH access, downloads ngrok, starts a TCP tunnel to port 22, and prints the SSH command in the workflow logs. The workflow waits until either the timeout expires or a `continue` file is created on the runner.

## Support

Please open a GitHub issue for bugs, questions, or security concerns. Pull requests are extremely welcomed!

This action is provided as-is and is intended for temporary debugging only. You are responsible for using it safely in your own workflows.

## FAQ

#### How to get ngrok auth token?

1. Go to https://ngrok.com/
2. Hit Sign up in the top right corner
3. Login via GitHub/Google or Sign up for a standalone account
4. From the given dashboard, you can now get your ngrok auth token

#### What regions are available for ngrok?

See https://ngrok.com/docs for latest information.

* us - United States
* eu - Europe
* ap - Asia/Pacific
* au - Australia
* sa - South America
* jp - Japan
* in - India

## License

This project is licensed under the GNU General Public License v3.0. See [COPYING](COPYING).
