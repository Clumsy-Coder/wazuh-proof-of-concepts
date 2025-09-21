# role detect-unauthorized-processes

Wazuh proof of concept to detect unauthorized processes

This role will:
- monitor process logs of a wazuh agent
  - restart **wazuh agent** service
- add rule to detect running unauthorized processes
  - programs
    - nc
    - nmap
  - restart **wazuh manager** service

## usage

Go to the root of the repo and run the following command

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/detect-unauthorized-processes.yaml
```

## Testing detecting unauthorized processes

You will need to SSH into a wazuh agent run an unauthorized program (`nc`, or `nmap`) for 30 seconds

```bash
nc -l 8000 & sleep 30 ; kill $!
```

or

```bash
nmap -sS <IP address> -T0 & sleep 35 ; kill $!
```

example

```bash
nmap -sS localhost -T0 & sleep 35 ; kill $!
```

After running the command, Wazuh will detect unauthorized processes are running and log them

### Screenshots

#### Running command `nc` on wazuh agent

##### Running unauthorized command `nc`for 35 seconds

![Running unauthorized command `nc` attempt](../../assets/images/detect-unauthorized-command-nc-attempt.png)

##### Wazuh dashboard detecting unauthorized command `nc`attempt

![Wazuh dashboard unauthorized command `nc` attempt](../../assets/images/detect-unauthorized-command-nc-Wazuh-daskboard.png)

---

#### Running command `nmap` on wazuh agent

##### Running unauthorized command `nmap `for 35 seconds

![Running unauthorized command `nmap` attempt](../../assets/images/detect-unauthorized-command-nmap-attempt.png)

##### Wazuh dashboard detecting unauthorized command `nmap `attempt

![Wazuh dashboard unauthorized command `nmap `attempt](../../assets/images/detect-unauthorized-command-nmap-Wazuh-daskboard.png)

## References

- https://documentation.wazuh.com/current/proof-of-concept-guide/detect-unauthorized-processes-netcat.html
