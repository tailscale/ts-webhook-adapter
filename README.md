# Tailscale Webhook Adapter

A simple service to receive incoming [webhook notifications from Tailscale](https://tailscale.com/kb/1213/webhooks/)
and reformat to be suitable for several popular services including:
- [Microsoft Teams](https://www.microsoft.com/en-us/microsoft-teams/group-chat-software)
- [Discord Forums](https://discord.com/)

----

## Tailscale Setup
Follow the instructions to [setup webhook notifications](https://tailscale.com/kb/1213/webhooks/),
and store the Secret as an environment variable named `TS_WEBHOOK_SECRET` for this service.

----

## Microsoft Teams
To forward notifications to Microsoft Teams, create a chanel within the destination
Team and choose Connectors. Ann an *Incoming Webhook*, and store the URL as an
environment variable named `TEAMS_WEBHOOK_URL` for this service.

![Teams Webhook configuration](images/Teams.png)

If no `TEAMS_WEBHOOK_URL` variable has been set, the Microsoft Teams delivery will be skipped.

----

## Discord
Webhook notifications can only be delivered to Discord Forum channels, which are only
available on Servers which have been set to Community mode.

After creating a Forum channel, choose *Integrations* > *Webhooks*. Store the URL
as an environment variable named `DISCORD_WEBHOOK_URL` for this service.

![Discord Webhook integration](images/Discord.png)

If no `DISCORD_WEBHOOK_URL` variable has been set, the Discord delivery will be skipped.

## To Run
Set up [Tailscale Funnel](https://tailscale.com/kb/1223/funnel). Once enabled then do the following to enable the endpoint:
1. Install [golang](https://go.dev/doc/install)
2. Clone this repo locally
3. cd into the cloned repo and run `go get` then `go build` to compile the ts--adapter
4. Ensure you've set your envioronment variables applicable above then run the newly compiled program: `./ts-webhook-adapter`
5. `tailscale funnel 8080` You will get a URL such as `https://<yourhostname>.examplemagic-dns.ts.net/`. Take note of this.
6. Go back to Tailscale Admin console and create a new Webhook Endpoint. Use `https://<yourhostname>.examplemagic-dns.ts.net/webhook` as the endpoint. Take note to add `/webhook` at the end of the URL.
7. Test the endpoint by click on the three-dots -> Test endpoint
You should begin receiving alerts now.
