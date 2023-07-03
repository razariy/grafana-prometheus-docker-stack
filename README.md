* Grafana: - <b>image: grafana/grafana:10.0.1-ubuntu</b>
* Prometheus: - <b>image: prom/prometheus:v2.45.0</b>
* Node-Exporter: - <b>image: prom/node-exporter:v1.6.0</b>

<h1>For deploying Grafana, Prometheus, and Node Exporter, take these steps:</h1>

<b>1) Clone repository or create a .yml file:</b>

- `git clone https://github.com/razariy/grafana-prometheus-docker-stack.git`

 or

- Create a new directory (for example: <b>"monitoring"</b>), then create a new file <b>docker-compose.yml</b> and copy the .yml code file from [HERE!!!](https://github.com/razariy/grafana-prometheus-docker-stack/blob/main/docker-compose.yml)

<b>2) Deploy stack:</b>
   
 - `sudo docker swarm init`
 - `sudo docker stack deploy -c grafana-docker-stack/docker-compose.yml monitoring`

 <b>3) After a few minutes, make sure that your docker containers are working:</b>

- `docker ps`
   
![image](https://github.com/razariy/grafana-prometheus-docker-stack/assets/79698042/1e098e59-6510-4c3f-913d-0a4dc2761b8e)


<b>4) Add Prometheus data source with address http://prometheus:9090 (or make use of IP-address your VM, where you're deployed environment) to grafana:</b>

![image](https://github.com/razariy/grafana-prometheus-docker-stack/assets/79698042/efb7592f-11d4-4781-8ac9-937f77ba76b9)
  
<b>5) Add these three lines to the bottom of `/var/lib/docker/volumes/monitoring_prom-configs/_data/prometheus.yml` file, `to scrape_configs`:</b>

> section:
>
>>     - job_name: 'node-exporter'
>> 
>>       static_configs:
>>         - targets: ['node-exporter:9100']

 
<b>6) Reload Prometheus config via this command:</b>
 - `docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP`


<b>7) Import this dashboard: https://grafana.com/grafana/dashboards/1860-node-exporter-full/ to grafana.</b>


<h3>That's it!</h3>


__________
> #### If you want to add more servers to prometheus, make these steps:
>
> Install node-exporter to each of these servers via these commands:
>>   -  `git clone https://github.com/razariy/grafana-prometheus-docker-stack.git`
>>   -  `docker stack deploy -c grafana-docker-stack/node-exporter.yml node-exporter`
>>> Add these servers to `/var/lib/docker/volumes/monitoring_prom-configs/_data/prometheus.yml` file:
>>>  - `job_name: 'node-exporter'`
>>>  - `targets: ['node-exporter:9100']` <b>section, like - targets: ['node-exporter:9100', 'server1:9100', 'server2:9100', '...']</b>
>>>
>>>> Reload Prometheus config via this command:
>>>> - `docker ps | grep prometheus | awk '{print $1}' | xargs docker kill -s SIGHUP`


<h6>The material was taken from digitalstudium </h6>
