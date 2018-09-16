commands to run

``docker network create --driver bridge consul-net``

``docker run -d --network=consul-net --name=consul1 -p 8300:8300 -p 8400:8400 -p 8500:8500 -p 8600:53/udp consul agent -server -bootstrap-expect=3 -ui -client=0.0.0.0 -node=consul1 -log-level=debug``

``docker run -d --network=consul-net --name=consul2 consul agent -server -retry-join consul1 -node=consul2 -log-level=debug``

``docker run -d --network=consul-net --name=consul3 consul agent -server -retry-join consul1 -node=consul3 -log-level=debug``

Congrats! You have Consul set up with a quorum! Usually you would just run one instance of this on host.

Now let's set up registrator to discover new services.

Since we're running consul on an internal network, we need to connect our containers to that bridge network.

``docker run -d --name=registrator --net=consul-net --volume=/var/run/docker.sock:/tmp/docker.sock gliderlabs/registrator:latest -internal consul://consul1:8500``


``docker run --name=haproxy-test --net=consul-net -p 8080:80 haproxy-app``

Since we're using registrator, we just rely on that to tell Consul where the microservice lives.

``docker run -d microservice1``

If you navigate to ``localhost:8080/hello-world``, you'll be able to see the hello world text from the microservice!