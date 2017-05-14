# nginx-proxy-saltstack-network-creation
Sample saltstack state to create a docker nginx-proxy bridge.

Please see nginx-proxy project and rational to create a dedicated docker network at https://github.com/jwilder/nginx-proxy

If you don't know what is *saltstack*, you probably lost your way when landing here, but you didn't lost your day!
Check about this amazing tool (which could looks like puppet, ansible, chef, ... in many ways) on this 10-minutes tutorial: https://docs.saltstack.com/en/latest/topics/tutorials/walkthrough.html

On one of your state file, just put such a state:
```yaml
# --subnet option set the subnet of this docker network (instead of random), to configure many hosts in the same way (iptables, ...)
# --opt com.docker.network.bridge.name option fix the real bridge name got from ```ip addr``` (instead of random name)
nginx-proxy-network:
  cmd.run:
    - names:
      - docker network create -d bridge --subnet 172.28.0.0/16 --opt com.docker.network.bridge.name=nginx-proxy nginx-proxy
    - unless: docker network list | grep nginx-proxy
```
