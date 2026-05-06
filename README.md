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

* Criação de wordlists de usuários e senhas
* Execução de ataque automatizado
* Validação de credenciais

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
* 2° passo: Definição do nível de segurança
* 3° passo: Automatizar de tentativas de login com o medusa

<img width="1232" height="675" alt="image" src="https://github.com/user-attachments/assets/17ef685d-3811-4043-94d0-28acdac10853" />

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

Testar autenticação utilizando uma senha comum para múltiplos usuários.

## 5.3.2 - Metodologia

* Enumeração de usuários
* Aplicação de password spraying
* Validação de acessos

## 5.3.3 - Execução


## 5.3.4 - Resultados

* Credenciais válidas encontradas
* Comportamento do sistema

---

## 🧠 Análise

* Diferença entre força bruta e password spraying
* Efetividade do ataque

---

# 🛡️ Medidas de Mitigação

Para evitar vulnerabilidades exploradas neste projeto, recomenda-se:

* Utilização de senhas fortes
* Implementação de autenticação multifator (MFA)
* Bloqueio após múltiplas tentativas de login
* Monitoramento de logs de acesso
* Implementação de rate limiting
* Uso de CAPTCHA em aplicações web

---

# 📸 Evidências

Adicione prints na pasta `/images` e referencie aqui:

* Enumeração de serviços
* Execução dos ataques
* Resultados obtidos

---

# 📁 Estrutura do Projeto

```id="u8"
projeto/
├── README.md
├── wordlists/
├── images/
```

---

# 📚 Aprendizados

Descreva aqui o que você aprendeu com o projeto.

Exemplo:

* Importância de políticas de senha
* Fragilidade de sistemas sem proteção
* Relevância de ferramentas clássicas de segurança

---

# ⚠️ Aviso Legal

Este projeto foi realizado exclusivamente para fins educacionais, em ambiente controlado, com sistemas vulneráveis intencionalmente configurados.

---

# 🚀 Conclusão

Resumo final do projeto e sua importância para a segurança da informação.

---
