# nginx-proxy-saltstack-network-creation
Sample saltstack state to create a docker nginx-proxy bridge.

Please discover nginx-proxy project and rational for creating a dedicated docker network at https://github.com/jwilder/nginx-proxy

If you don't know what is *saltstack*, you probably lost your way when landing here, but you didn't lost your day!
Check about this amazing tool (which could looks like puppet, ansible, chef, ... in many ways) on this 10-minutes tutorial: https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html

On one of your state files loaded for your target hosts, just drop such a state:
```yaml
# --subnet option set the subnet of this docker network (instead of random), to configure many hosts in the same way (iptables, ...)
# --opt com.docker.network.bridge.name option fix the real bridge name got from ```ip addr``` (instead of random name)
nginx-proxy-network:
  cmd.run:
    - names:
      - docker network create -d bridge --subnet 172.28.0.0/16 --opt com.docker.network.bridge.name=nginx-proxy nginx-proxy
    - unless: docker network list | grep nginx-proxy
```

# Resources about nginx-proxy and saltstack:

For further information regarding the various components around nginx-proxy and saltstack, please refer to each project:

## Regarding nginx-proxy
* [The official NGINX image](https://github.com/nginxinc/docker-nginx)
* [JWilder's docker-gen](https://github.com/jwilder/docker-gen)
* [JrCs' docker-letsencrypt nginx-proxy companion ](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)
* [Ekkis' docker-compose sample bundle to automate the letsencrypt-companion/nginx-proxy integration](https://github.com/ekkis/nginx-proxy-LE-docker-compose)

## Regarding saltstack
* [The official SaltStack documentation](https://docs.saltstack.com/en/latest/)
* [A straight forward quickstart to discover saltstack](https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html)
