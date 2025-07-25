Execute the following command to pull the image:

```bash
docker pull thingsboard/tb-pe-http-integration:{{ site.release.pe_full_ver }}
```
{: .copy-code}

Execute the following command to create volume for the integration logs (799 is the user id of ThingsBoard non-root docker user):

```bash
mkdir -p ~/.tb-pe-http-integration-logs && sudo chown -R 799:799 ~/.tb-pe-http-integration-logs
```
{: .copy-code}

Execute the following command to run the integration:

```bash
docker run -it -p 8082:8082 -v ~/.tb-pe-http-integration-logs:/var/log/tb-http-integration \
-e "RPC_HOST=thingsboard.cloud" -e "RPC_PORT=9090" \
-e "INTEGRATION_ROUTING_KEY=YOUR_ROUTING_KEY"  -e "INTEGRATION_SECRET=YOUR_SECRET" \
--name my-tb-pe-http-integration --restart always thingsboard/tb-pe-http-integration:{{ site.release.pe_full_ver }}
```
{: .copy-code}

Where: 
    
- `thingsboard.cloud` - is the host name of your ThingsBoard PE instance;
- `9090` - is the port of your ThingsBoard PE instance. It is configured in thingsboard.yml using INTEGRATIONS_RPC_PORT env variable;    
- `YOUR_ROUTING_KEY` - placeholder for your **integration key** obtained on [create remote integration in ThingsBoard](#create-remote-integration-in-thingsboard);
- `YOUR_SECRET` - placeholder for your **integration secret** obtained on [create remote integration in ThingsBoard](#create-remote-integration-in-thingsboard);
- `docker run`              - run this container;
- `-it`                     - attach a terminal session with current ThingsBoard process output;
- `-v ~/.tb-pe-http-integration-logs:/var/log/tb-http-integration`   - mounts the host's dir `~/.tb-pe-http-integration-logs` to ThingsBoard logs directory;
- `--name tb-pe-http-integration`             - friendly local name of this machine;
- `--restart always`        - automatically start ThingsBoard Integration in case of system reboot and restart in case of failure.;
- `thingsboard/tb-pe-http-integration:{{ site.release.pe_full_ver }}`          - docker image.

After executing this command you can open logs which are located here `~/.tb-pe-http-integration-logs`. 
You should see some INFO log messages with your latest Integration configuration that arrived from the server.

{% capture difference %}
You can detach from session terminal with **`Ctrl-p`**+**`Ctrl-q`** - the container will keep running in the background.
{% endcapture %}
{% include templates/info-banner.md content=difference %}

- **Reattaching, stop and start commands**

To reattach to the terminal (to see ThingsBoard logs) run:

```
docker attach tb-pe-http-integration
```
{: .copy-code}

To stop the container:

```
docker stop tb-pe-http-integration
```
{: .copy-code}

To start the container:

```
docker start tb-pe-http-integration
```
{: .copy-code}