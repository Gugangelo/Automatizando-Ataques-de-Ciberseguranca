# Automatizando-Ataques-de-Ciberseguranca
Este projeto de estudos em cibersegurança utiliza o Kali Linux para configurar um cenário de ataque controlado contra um sistema Metasploitable 2, concentrando-se no uso estratégico do Medusa para executar ataques de Força Bruta em cenários práticos envolvendo serviços FTP, aplicações Web vulneráveis e autenticação SMB. Explorando e documentando o impacto de senhas fracas e como mitigá-lo.

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

## 📌 Objetivo

Realizar tentativa de autenticação via força bruta no serviço FTP do sistema.

## ⚙️ Metodologia

* Criação de wordlists de usuários e senhas
* Execução de ataque automatizado
* Validação de credenciais

---

## 💻 Execução

<img width="581" height="38" alt="image" src="https://github.com/user-attachments/assets/ecdd1c4f-47e0-4092-913e-bbf2cb048163" />

Este comando usa várias combinações de usuário + senha baseados nas wordlists inseridas em -U (para usuários) e -P (para senhas) contra um serviço FTP em um alvo específico, ou seja, simula um ataque de força bruta em FTP. Assim executando uma permutação das 2 wordlists, como mostra a imagem abaixo. 

<img width="631" height="476" alt="image" src="https://github.com/user-attachments/assets/4aee682f-c4f2-4651-9075-9b5aab9f2eec" />

---

## 📊 Resultados

* Credenciais encontradas:

```id="u4"
(adicione aqui)
```

* Tempo de execução:

```id="u5"
(adicione aqui)
```

---

## ✅ Validação

Descreva aqui como você confirmou o acesso ao serviço.

---

## 🧠 Análise

Explique:

* Por que o ataque funcionou
* Quais vulnerabilidades estavam presentes

---

# 🌐 Cenário 2: Ataque em Aplicação Web (DVWA)

## 📌 Objetivo

Simular ataque de força bruta em formulário de login web.

---

## ⚙️ Metodologia

* Configuração do DVWA
* Definição do nível de segurança
* Automação de tentativas de login

---

## 💻 Execução

```id="u6"
(comandos ou ferramenta utilizada)
```

---

## 📊 Resultados

* Sucesso ou falha do ataque
* Diferença entre níveis de segurança

---

## 🧠 Análise

Explique:

* Comportamento do sistema
* Existência de proteções (ou ausência delas)

---

# 🧠 Cenário 3: Password Spraying em SMB

## 📌 Objetivo

Testar autenticação utilizando uma senha comum para múltiplos usuários.

---

## ⚙️ Metodologia

* Enumeração de usuários
* Aplicação de password spraying
* Validação de acessos

---

## 💻 Execução

```bash id="u7"
(comando utilizado)
```

---

## 📊 Resultados

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
