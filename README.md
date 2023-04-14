# aap-eda-demo
Ansible Demonstration of Event Driven Automation

Prereqs:

`dnf --assumeyes install java-17-openjdk python3-pip`  

`export JAVA_HOME=/usr/lib/jvm/java-17-openjdk`  

`pip install ansible-rulebook`  

`ansible-galaxy collection install ansible.eda`  


## Webhook example
on terminal 1  
`ansible-rulebook --rulebook webhook-example.yml -i inventory.yml --print-events`  

on terminal 2  
`curl -H 'Content-Type: application/json' -d "{\"message\": \"Ansible is alright\"}" 127.0.0.1:5000/endpoint`  

look at results on terminal 1  

on terminal 2  
`curl -H 'Content-Type: application/json' -d "{\"message\": \"Ansible is super cool\"}" 127.0.0.1:5000/endpoint`  

look at results on terminal 1

## Configuration Drift Example (webhook)
on terminal 1  
`ansible-rulebook --rulebook webhook-example.yml -i inventory.yml --print-events`  

on terminal 2 (to test)
`curl -H 'Content-Type: application/json' -d "{\"event_type\": \"drift-baseline-detected\"}" 127.0.0.1:5000/endpoint`  
(or public URL from outside)  

output should be:  

```
{   'meta': {   'endpoint': 'endpoint',
                'headers': {   'Accept': '*/*',
                               'Content-Length': '41',
                               'Content-Type': 'application/json',
                               'Host': 'aap.leonlevy.net:5000',
                               'User-Agent': 'curl/7.87.0'}},
    'payload': {'event_type': 'drift-baseline-detected'}}

PLAY [Remediation Playbook - Restore Baseline] *********************************

TASK [debug] *******************************************************************
ok: [localhost] => {
    "msg": "Running Remediation Tasks for System!"
}
```    

To test webhook with insights, set up basline config drift monitoring and set up webhook integrations

Using Red Hat Insights as a source of events for Event-Driven Ansible automation  
https://www.ansible.com/blog/using-red-hat-insights-as-a-source-of-events-for-event-driven-ansible-automation  

Introducing Red Hat Insights Drift Capability for Red Hat Enterprise Linux configuration troubleshooting
https://www.redhat.com/en/blog/introducing-red-hat-insights-drift-capability-red-hat-enterprise-linux-configuration-troubleshooting


Trigger event on managed RHEL system by issuing the following command on it:


`sudo insights-client`
