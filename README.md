# Automatizando-Ataques-de-Ciberseguranca
Este projeto de estudos em cibersegurança utiliza o Kali Linux para configurar um cenário de ataque controlado contra um sistema Metasploitable 2 e uma aplicação web DVWA, concentrando-se no uso estratégico do Medusa para executar ataques de Força Bruta em cenários práticos envolvendo serviços FTP, aplicações Web vulneráveis e autenticação SMB. Explorando e documentando o impacto de senhas fracas e como mitigá-lo.

---

# 1 - Objetivos

* Compreender o funcionamento de ataques de força bruta
* Identificar vulnerabilidades em sistemas mal configurados
* Propor medidas de mitigação
* Documentar processos técnicos de forma clara

---

# 2 - Ambiente de Testes

| Componente       | Descrição        |
| ---------------- | ---------------- |
| Sistema atacante | Kali Linux       |
| Sistema alvo     | Metasploitable 2 |
| Virtualização    | VirtualBox       |
| Rede             | Host-Only        |

---

# 3 - Enumeração de Serviços Disponíveis

Ferramenta utilizada: Nmap

### Resultados:

<img width="800" height="340" alt="image" src="https://github.com/user-attachments/assets/726ac31a-121a-4ad8-baa4-04b36eb2d4c1" />

O comando ping é usado para testar a conectividade entre o sistema utilizado e o sistema alvo em uma rede IP.

📌 Observação:

| Porta | Serviço     | Status |
| ----- | ----------- | ------ |
| 21    | FTP         | Aberto |
| 80    | HTTP (DVWA) | Aberto |
| 445   | SMB         | Aberto |

Pode-se notar que, além das portas estarem todas abertas (visto que trata-se de um sistema Metasploitable 2, feito para apresentar tais vulnerabilidades), pode se identificar o sistema como Linux.

---

# 4 - Definição de Wordlists

Uma wordlist é um arquivo de texto contendo palavras, frases, números ou combinações de caracteres, comumente usada em segurança da informação para testes de força bruta (brute force) e ataques de dicionário para adivinhar senhas.
Para definir os nomes de usuários e senhas que serão guardadas nas wordlists e usadas para os testes, tomei a liberdade de pesquisar pelos 10 nomes de usuário e senhas mais comuns de serem utilizados, usando como fonte os seguintes sites:
- Nomes de usuários mais comuns: https://isc.sans.edu/diary/30188
- Senhas mais comuns: https://www.tecmundo.com.br/seguranca/409382-nordpass-revela-lista-de-senhas-mais-usada-no-brasil-em-2025.htm

### Comando para criação de Wordlist

<img width="636" height="125" alt="image" src="https://github.com/user-attachments/assets/4cd093a8-5a89-42fc-a691-2316201747e7" />

O comando acima cria os arquivos de strings utilizadas no formato txt. Ex: *echo -e "123456\nsenha" > password.txt* cria um arquivo chamado password.txt que possui 2 strings, "123456" e pula uma linha (\n) para digitar "senha".

---

# 5 - Testes

# 5.1 - Cenário 1: Ataque de Força Bruta em FTP

## 5.1.1 - Objetivo

Realizar tentativa de autenticação via força bruta no serviço FTP do sistema Metasploitable 2.

## 5.1.2 - Metodologia

* 1° passo: Criação de wordlists de usuários e senhas.
* 2° passo: Execução de ataque automatizado.
* 3° passo: Checagem de credenciais obtidas.

## 5.1.3 - Execução

<img width="581" height="38" alt="image" src="https://github.com/user-attachments/assets/ecdd1c4f-47e0-4092-913e-bbf2cb048163" />

Este comando usa várias combinações de usuário + senha baseados nas wordlists inseridas em -U (para usuários) e -P (para senhas) contra um serviço FTP em um alvo específico, ou seja, simula um ataque de força bruta em FTP. Assim executando uma permutação das 2 wordlists, como mostra a imagem abaixo. 

<img width="631" height="476" alt="image" src="https://github.com/user-attachments/assets/4aee682f-c4f2-4651-9075-9b5aab9f2eec" />

---

## 5.1.4 - Resultados

* Credenciais encontradas:
Dos 100 testes realizados pela permutação de 10 pra 10 elementos nas 2 wordlists, pudemos localizar 1 conta com acesso válido encontrada:
#### usuário: test
#### senha: 123456

<img width="644" height="82" alt="image" src="https://github.com/user-attachments/assets/d7ea2155-68bf-43a6-859e-9e76eb2978d7" />

É importante notar que, além de representar uma conta padrão com nome e senhas muito repetidos, principalmente em SO Linux, este teste, assim como os próximos, representa uma abstração de um processo de hacking real, devido ao número de elementos utilizados (apenas 10 nomes de usuário e senhas). Em um processo real centenas de milhares de nomes e senhas poderiam ser testados para gerar bilhões de acessos diferentes. 

---

# 5.2 - Cenário 2: Ataque em Aplicação Web (DVWA)

## 5.2.1 - Objetivo

Simular um ataque de força bruta em um formulário de login na web usando o DVWA (Damn Vulnerable Web Applications), uma ferramenta8 que contém diversos tipos de vulnerabilidades Web, sendo assim uma ótima solução para praticar conhecimentos em Segurança de Aplicações Web.

## 5.2.2 - Metodologia

* 1° passo: Acessar a URL http://<IP_DO_METASPLOITABLE>/dvwa

<img width="1232" height="675" alt="image" src="https://github.com/user-attachments/assets/17ef685d-3811-4043-94d0-28acdac10853" />

* 2° passo: Definição do nível de segurança para "low".
* 3° passo: Desbrindo os parâmetros utilizados no processo de login com inspecionar elementos.

<img width="1243" height="688" alt="image" src="https://github.com/user-attachments/assets/6cdc88ef-cf9b-4d0d-b0d0-a6c3c4d90704" />

* 4° passo: Automatizar de tentativas de login com o medusa.

## 5.2.3 - Execução

<img width="850" height="80" alt="image" src="https://github.com/user-attachments/assets/f9a8aa4d-11d3-47dc-9de4-d47f33207d56" />
Este comando executa um ataque de força bruta via HTTP no DVWA automatizando tentativas de login. o -M http define o uso do módulo HTTP para configurar este comando como um ataque em aplicação web. O grep serve para mostrar apenas linhas que deram um resultado de sucesso, deixando assim o resulttado do código mais limpo.

<img width="921" height="48" alt="image" src="https://github.com/user-attachments/assets/d654e095-5ecf-4634-831c-01e19d4a3a48" />
O hydra é uma ferramenta de força bruta muito similar ao medusa, mas com foco mais direto para formulários web. A diferença mais notória entre eles é a adição de "http-post-form", que permite explicitar mais diretamente a forma de resposta http requerida, além de por padrão utilizar 16 threads. A razão de seu uso neste cenário será explicado mais adiante. 

## 5.2.4 - Resultados

### 1° Teste: Medusa

<img width="919" height="595" alt="image" src="https://github.com/user-attachments/assets/cbdeb47c-7fce-446d-af41-8911e7c16892" />

Teste completo sem filtragem de resulttados positivos.

<img width="915" height="251" alt="image" src="https://github.com/user-attachments/assets/6281dd46-63a2-44da-a2d2-8fe1231e982b" />

Durante este teste foi observado que o medusa não se comportou de maneira ideal para a situação de um formulário http. Apenas 10 testes foram realizados com 1 senha, mesmo com os parâmetros corretos inseridos. Além disso ele marcou todos os resultados como sucesso mesmo que com testes seja apontado que nenhum desses resultados representa um usuário cadastrado no cinema.

### 2° Teste: Hydra

<img width="916" height="205" alt="image" src="https://github.com/user-attachments/assets/d31efcb3-72e9-487e-8519-3fa01c71b9f6" />

Por outro lado o hydra conseguiu realizar todos os testes, entretanto o resultado final foi de nenhumas das 100 combinações representando as credenciais de um usuário cadastrado no sistema DVWA.

Visto esse resultado, eu preferi manter a wordlist de usuários mas procurar por uma lista com mais elementos para a wordlist de senhas. A lista utilizada é uma já vem instalada dentro do Kali Linux, a "unix_passwords.txt" é uma wordlist que contém 1021 senhas populares em sistemas Linux, gerando assim mais de 10000 combinações de senha + usuário. Após isso o seguinte resultado foi observado: 

<img width="921" height="298" alt="image" src="https://github.com/user-attachments/assets/8766c29d-c2e6-486c-85d9-bc5b62364d9f" />

É visível que uma credencial válida foi encontrada, com mais de 10000 testes feitos em 16 threads simultâneas resultando em um tempo de duração por volta de 5 minutos, o hydra se mostrou verdadeiramente eficaz para o cenário de ataque em um formulário de login web.

---

# 5.3 - Cenário 3: Password Spraying em SMB

## 5.3.1 - Objetivo

Testar autenticação utilizando poucas senha comum para múltiplos usuários. Mirando no serviço SMB e usando enumeração para, entre diversos dados, determinar usuários válidos do IP alvo e assim usar poucas senhas populares em comum para encontrar credenciais válidas.

## 5.3.2 - Metodologia

* 1° passo: Enumeração de usuários usando enum.
* 2° passo: Aplicação de password spraying com o medusa.
* 3° passo: Validação das credencias obtidas.

## 5.3.3 - Execução

<img width="494" height="39" alt="image" src="https://github.com/user-attachments/assets/735bb404-bdc7-47d4-9d36-79143dbe8e29" />

O código acima é usado para enumerar diversas coisas sobre o IP alvo, com o parâmetro -a servindo para utilizar todas as ferramentas de listagem possíveis, enquanto o "tee" registra todos esses dados em um arquivo.

<img width="261" height="579" alt="image" src="https://github.com/user-attachments/assets/48d04b19-8cc5-4237-9180-91fd7a9b2258" />

Baseado nos usuários obtidos alguns dos nomes mais comuns e visados foram escolhidos para criar uma nova wordlist de usuários.

<img width="812" height="50" alt="image" src="https://github.com/user-attachments/assets/4ade98a8-5789-46f5-b66a-79a0a9a83c41" />

E por fim, para as senhas que serão utilizadas foram escolhidas apenas 5 das senhas mais populares utilizadas.

<img width="611" height="44" alt="image" src="https://github.com/user-attachments/assets/e94a8d6d-7882-440c-a32d-1b5b6d73ec28" />

## 5.3.4 - Resultados

Antes de tudo é importante elucidar alguns dos parâmetros usados nesse comando como -M smbnt que seleciona o módulo de ataque especificamente para quebrar senhas do protocolo SMB, -T 50 que define o número total de alvos a serem testados simultaneamente (neste caso, embora haja apenas um host, o parâmetro limita o paralelismo global). Apenas os resultados positivos obtidos foram mostrados a fim de manter clareza.

<img width="917" height="77" alt="image" src="https://github.com/user-attachments/assets/33572c51-7254-4ff6-acd4-e76f2118e062" />

#### usuário: msfadmin
#### senha: msfadmin

Após os testes foi encontrada uma credencial válida, mas antes ela precisa ser testada para advocar sua validez.

<img width="754" height="333" alt="image" src="https://github.com/user-attachments/assets/728eed85-339e-4f34-9d21-d17a5d9b5fc6" />

Para confirmar as credenciais obtidas anteriormente, usamos do comando "smbclient", assim confirmando sua validez.

---

# 6 - Conclusão

A execução deste projeto evidenciou que vulnerabilidades críticas ainda podem ser exploradas a partir de técnicas simples, como ataques de força bruta, especialmente em ambientes que não implementam boas práticas de segurança. É importante citar que os testes realizados representam versões menores de uma real tentativa de invasão, que usaria milhões de usuários e milhões de senhas para criar um número imenso de combinações a fim de invadir o sistema alvo, enquanto os testes utilizaram apenas uma base reduzida de dados, esses nomes que compõe as wordlists advém justamente de vazamentos, o que comprova mais o ponto de que há uma necessidade grande por cibersegurança.
Dessa forma, conclui-se que a segurança de sistemas depende diretamente da adoção de práticas como políticas de senha robustas, autenticação multifator, monitoramento contínuo, limitação de tentativas de login e uso de CAPTCHA em aplicações web, deve todas ser aplicadas para prevenir danos e vazamentos de dados que tem potencial desastroso. Este projeto reforça que o conhecimento das técnicas ofensivas é fundamental para a construção de defesas eficazes, sendo essencial para profissionais que atuam na área de segurança da informação.
