# aap-eda-demo
Ansible Demonstration of Event Driven Automation

Prereqs

dnf --assumeyes install java-17-openjdk python3-pip

export JAVA_HOME=/usr/lib/jvm/java-17-openjdk

pip install ansible-rulebook

ansible-galaxy collection install ansible.eda  

For fix-web example you need to have httpd installed on a host, and update /etc/hosts with an ip address for `webserver`  

## Webhook example
on terminal 1  
`ansible-rulebook --rulebook webhook-example.yml -i inventory.yml --print-events`  

on terminal 2  
`curl -H 'Content-Type: application/json' -d "{\"message\": \"Ansible is alright\"}" 127.0.0.1:5000/endpoint`  

look at results on terminal 1  

on terminal 2  
`curl -H 'Content-Type: application/json' -d "{\"message\": \"Ansible is super cool\"}" 127.0.0.1:5000/endpoint`  

look at results on terminal 1

## URL Check example
on terminal 1  
`ansible-rulebook --rulebook url-check.yml -i inventory_web.yml --print-events`  

on terminal 2
`curl webserver`  

output should be:  

```
<!DOCTYPE html>
<html>
<body>

<h1>Sample Page for EDA</h1>
<p>This is a basic page - WOOO HOOOOO!</p>

</body>
</html>
```    
on webserver  

`sudo rm /var/www/html/index.html` 






