apache_test
=========

A simple Ansible playbook to install Apache httpd along with a simple Index html file in its web root.

Requirements
------------

- Target node/machine should be RHEL 6, 7 with Python installed on it
- Ensure AWS Security Group Rules are set to access the Instance on HTTP port

Ansible Extra Arguments
-----------------------

To run the Ansible playbook, few modifications might be required depending on your environment.

Based on the tests conducted so far, here are the recommendations to run ansible playbook:

* Use SSH key based authentication. Pass "--private-key" argument with "ansible-playbook" command
* Update Inventory file "inventory" according to your environment.
* Use -t --tag switch on ansible-playbook to run specific scenario
* 2 Scenarios are available

* -t setup_httpd = Install httpd, add custom index page, Firewall rule to allow HTTP port, Start httpd service
* -t test_httpd = Performs HTTP GET on Index page connecting to Instance on HTTP port, print "Successful" message if GET operation returns 200 response code

Sample Run
----------

Update Inventory
----------------

[ec2]
ec2-52-77-245-101.ap-southeast-1.compute.amazonaws.com

Pre Check
---------

ansible all -i inventory -m ping --private-key=../aws_ec2/rd-sing-kp3.pem

# Output:

```
ec2-52-77-245-101.ap-southeast-1.compute.amazonaws.com | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/libexec/platform-python"
    },
    "changed": false,
    "ping": "pong"
}
```

Playbook Run
------------

Scenario #1:

```
ansible-playbook -i inventory site.yml -t setup_httpd --private-key=../aws_ec2/rd-sing-kp3.pem
```

```
# Output (Extract):
PLAY RECAP ********************************************************************************************************************************************
ec2-52-77-245-101.ap-southeast-1.compute.amazonaws.com : ok=10   changed=6    unreachable=0    failed=0    skipped=2    rescued=0    ignored=0
```

Scenario #2:

```
ansible-playbook -i inventory site.yml -t test_httpd --private-key=../aws_ec2/rd-sing-kp3.pem
```

```
TASK [apache_test : Verify HTTP response code] ********************************************************************************************************
ok: [ec2-52-77-245-101.ap-southeast-1.compute.amazonaws.com] => {
    "msg": "HTTP GET on Index Page Successful"
}

PLAY RECAP ********************************************************************************************************************************************
ec2-52-77-245-101.ap-southeast-1.compute.amazonaws.com : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

License
-------

BSD

Author Information
------------------

Raj Durvasula

