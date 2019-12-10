[defaults]
inventory=./inventory
remote_user=root
host_key_checking=False
forks=30

gathering=smart
gather_facts=True
fact_caching = jsonfile
fact_caching_timeout = 3600
fact_caching_connection = /tmp/ansible/facts/cache 

[ssh_connection]
pipelining = True
; ssh_args=-o ForwardAgent=yes  -o ControlMaster=auto -o ControlPersist=60s -o "ProxyCommand ssh -A -i ~/.ssh/id_rsa root@47.100.214.33 nc %h %p"
ssh_args=-o ForwardAgent=yes  -o ControlMaster=auto -o ControlPersist=60s
control_path = /tmp/ansible-ssh-%%h-%%p-%%r
