1. Pull container
2. Create Dockerfile
3. Create dnsmasq.conf file
4. Run Container
5. Test

Create a /opt/dnsmasq.conf file 

    #dnsmasq config, for a complete example, see: \
    #http://oss.segetech.com/intra/srv/dnsmasq.conf \
    #log all dns queries \
    log-queries \
    #dont use hosts nameservers \
    no-resolv \
    #use cloudflare as default nameservers, prefer 1^4 \
    server=1.0.0.1 \
    server=1.1.1.1 \
    strict-order \
    #serve all .company queries using a specific nameserver \
    server=/company/10.0.0.1 \
    #explicitly define host-ip mappings \
    address=/myhost.company/10.0.0.2 \
    
Run the container
    
    $ docker run \
    --name dnsmasq \
    -d \
    -p 53:53/udp \
    -p 5380:8080 \
    -v /opt/dnsmasq.conf:/etc/dnsmasq.conf \
    --log-opt "max-size=100m" \
    -e "HTTP_USER=foo" \
    -e "HTTP_PASS=bar" \
    --restart always \
    jpillora/dnsmasq
    
Visit http://\<docker-host>\:5380, authenticate with foo/bar and you should see
