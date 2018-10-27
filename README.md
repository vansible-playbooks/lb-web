### Pre-requisites
    Local MAC/system is the management node
    Has Ansible installed(Chosen config management tool)

### Nginx as "back end web servers"
    - Setup Nginx as webserver
		web1.nginx
		web2.nginx
		web3.nginx

### HAProxy as "load balancer" and "front end proxy"
    - Setup HAProxy as load balancer
		lb.haproxy

##### Steps to bring up the set up 
  > vagrant up

##### Access the application
  - VIA HAProxy as the Front end and Load Balancer
    http://lb.haproxy:9095

  - To view the stats
    http://lb.haproxy:9095?haproxy/stats

	- VIA each individual web server node
		http://web1.nginx
		http://web2.nginx
    http://web3.nginx

##### Check status
		http://rp1.nginx:8080/nginx_status
		http://rp2.nginx:8080/nginx_status
		http://vip.nginx:8080/nginx_status

#### Tested with vagrant version 1.9.4 and virtualbox 5.1.22r115126
