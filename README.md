# 📝 Relatório de Auditoria: Ataque de Força Bruta

**Projeto:** Desafio DIO - Brute Force com Kali Linux e Medusa
**Autor:** Paulo Henrique
**Data:** 04 de Novembro de 2025

---

## 1. Introdução

Este documento resume a execução do desafio prático de cibersegurança, onde simulei ataques de força bruta contra um ambiente intencionalmente vulnerável, o Metasploitable 2. O objetivo foi aplicar o conhecimento adquirido nas aulas para entender a mecânica dos ataques e desenvolver uma mentalidade de defesa (Blue Team).

O foco da auditoria foi a exploração de credenciais fracas ou padrão em serviços de rede e formulários web, utilizando as ferramentas **Medusa** e **Hydra**.

## 2. Configuração do Laboratório

O ambiente foi montado com sucesso no VirtualBox, usando uma rede Host-Only para garantir o isolamento total.

* **Máquina Atacante:** Kali Linux (192.168.56.100)
* **Máquina Alvo:** Metasploitable 2 (192.168.56.101)

A primeira etapa, a **enumeração**, utilizando `nmap`, confirmou a presença dos principais serviços a serem auditados: FTP (21), SMB (445) e o servidor web (80) que hospeda o DVWA.

## 3. Sumário dos Ataques e Descobertas

### A. Força Bruta Clássica (FTP)

* **Ferramenta:** Medusa.
* **Descoberta:** O ataque foi bem-sucedido rapidamente devido ao uso de senhas fracas e padrão (`msfadmin` / `msfadmin`). O serviço FTP não implementa nenhum mecanismo de *rate limiting* ou bloqueio de tentativas.
* **Vulnerabilidade:** Credenciais fracas, falta de controle de acesso.

### B. Password Spraying (SMB)

* **Ferramenta:** Medusa.
* **Descoberta:** A técnica de *password spraying* foi aplicada para testar uma única senha comum em uma lista de usuários, simulando um ataque que tenta evitar o bloqueio de contas individuais. A ausência de bloqueio no Metasploitable 2 permitiu a validação da técnica.
* **Vulnerabilidade:** Ameaça alta quando senhas comuns são utilizadas em massa (ex: "Trocar123").

### C. Formulário Web (DVWA)

* **Ferramenta:** Hydra.
* **Descoberta:** O Hydra foi eficaz em automatizar o processo de login HTTP POST, aproveitando a total falta de proteção do DVWA (como CAPTCHA ou tokens anti-CSRF).
* **Vulnerabilidade:** Formulários web desprotegidos contra automação (bots).

## 4. Recomendações de Segurança (Mitigação)

Os testes confirmam que a maior parte dos serviços seria vulnerável a ataques de força bruta em um ambiente real. As seguintes medidas são urgentes:

1.  **Implantar MFA (Autenticação Multifator):** A solução mais eficaz, pois a senha sozinha não garante mais o acesso.
2.  **Utilizar Fail2Ban:** Configurar essa ferramenta em serviços como FTP e SSH para bloquear automaticamente o endereço IP de origem após três a cinco tentativas de login fracassadas.
3.  **Auditoria de Senhas:** Forçar o uso de senhas complexas e, idealmente, exigir que os usuários troquem as senhas fracas descobertas.

## 5. Conclusão da Jornada

O desafio foi fundamental para solidificar a diferença entre as ferramentas (Medusa para protocolos, Hydra para web) e, mais importante, para internalizar a mentalidade de que a prevenção (Defesa) sempre deve vir após a descoberta da vulnerabilidade (Ataque). O projeto atua como um excelente portfólio de competências técnicas e analíticas.
