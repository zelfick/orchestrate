


#### JSON Configuration File
The Vagrantfile retrieves multiple VM configurations from a separate `node.json` file. All VM configuration is
contained in the JSON file. You can add additional VMs to the JSON file, following the existing pattern. The
Vagrantfile will loop through all nodes (VMs) in the `node.json` file and create the VMs. You can easily swap
configuration files for alternate environments since the Vagrantfile is designed to be generic and portable.

#### Instructions
```
vagrant up # brings up all VMs
vagrant ssh puppet.example.com

sudo service puppetmaster status # test that puppet master was installed
sudo service puppetmaster stop
sudo puppet master --verbose --no-daemonize
# Ctrl+C to kill puppet master
sudo service puppetmaster start
sudo puppet cert list --all # check for 'puppet' cert

# Shift+Ctrl+T # new tab on host
vagrant ssh node01.example.com # ssh into agent node
sudo service puppet status # test that agent was installed
sudo puppet agent --test --waitforcert=60 # initiate certificate signing request (CSR)
```
Back on the Puppet Master server (puppet.example.com)
```
sudo puppet cert list # should see 'node01.example.com' cert waiting for signature
sudo puppet cert sign --all # sign the agent node(s) cert(s)
sudo puppet cert list --all # check for signed cert(s)
```
#### Forwarding Ports
Used by Vagrant and VirtualBox. To create additional forwarding ports, add them to the 'ports' array. For example:
 ```
 "ports": [
        {
          ":host": 1234,
          ":guest": 2234,
          ":id": "port-1"
        },
        {
          ":host": 5678,
          ":guest": 6789,
          ":id": "port-2"
        }
      ]
```
