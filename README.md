# 🛡️ Laboratório de Cibersegurança: Simulação de Malwares com Python

⚠️ **AVISO DE USO ÉTICO E RESPONSÁVEL** ⚠️  
Este repositório e seu conteúdo são destinados estritamente para fins educacionais e de pesquisa em cibersegurança. Os scripts aqui presentes simulam o comportamento de malwares (Ransomware e Keylogger) em um ambiente controlado para ajudar a entender seu funcionamento, e não devem ser usados para atividades maliciosas.  
O autor e contribuintes não se responsabilizam pelo mau uso das informações ou dos códigos aqui apresentados. Use-os por sua conta e risco, apenas em sistemas que você possui e com permissão explícita.

---

## 1. Visão Geral do Projeto

Este projeto é um laboratório prático que demonstra o funcionamento interno de duas das ameaças digitais mais comuns: Ransomware e Keylogger. Utilizando Python, criamos simulações seguras para dissecar como esses malwares operam, desde a infecção e captura de dados até a exfiltração de informações.  

O objetivo principal é aprender "atacando" (Red Team) para saber como se "defender" (Blue Team) de forma mais eficaz.  

### Objetivos de Aprendizagem
- Compreender o mecanismo de criptografia de um Ransomware.
- Entender como um Keylogger captura e armazena (ou envia) dados.
- Configurar um ambiente de laboratório seguro para testes.
- Refletir sobre estratégias de defesa, detecção e mitigação.

---

## 2. Pré-requisitos

Para replicar este laboratório, você precisará ter o Python 3 instalado, juntamente com algumas bibliotecas.

```bash
# Clone este repositório (ou crie os arquivos manualmente)
git clone https://github.com/andersonfreire/lab_ciberseguranca.git
cd lab_ciberseguranca

# Instale as bibliotecas necessárias
pip install cryptography pynput
```
---

## 3. Simulação 1: Ransomware

Um Ransomware é um malware que "sequestra" os arquivos da vítima, criptografando-os e tornando-os inacessíveis. Em seguida, exige um pagamento (resgate), geralmente em criptomoeda, para fornecer a chave de descriptografia.

### 3.1. Arquivos da Simulação

- `ransoware.py`: O script "atacante". Ele gera uma chave, procura arquivos-alvo e os criptografa.
- `descriptografa.py`: O script de "resgate". Ele usa a chave gerada para reverter o processo e descriptografar os arquivos.
- `dados_confidenciais.txt` / `senhas.txt`: Arquivos de exemplo que servirão como "vítimas".

### 3.2. Configurando o Ambiente do Laboratório

Para evitar danos acidentais, **JAMAIS** execute este script em seu diretório raiz ou em pastas com arquivos importantes. Vamos criar um "sandbox" (ambiente controlado) para ele.

1. Crie uma pasta principal para o laboratório, por exemplo: `lab_ciberseguranca`.
2. Dentro dela, coloque os scripts `ransoware.py` e `descriptografa.py`.
3. Crie uma subpasta chamada `test_files`.
4. Mova os arquivos `dados_confidenciais.txt` e `senhas.txt` para dentro da pasta `test_files`.

Sua estrutura de pastas deve ficar assim:
```bash
lab_ciberseguranca/
├── ransoware.py
├── descriptografa.py
└── test_files/
    ├── dados_confidenciais.txt
    └── senhas.txt
```
### 3.3. Executando a Simulação

**Passo 1: O Ataque**

No terminal, navegue até a pasta `lab_ciberseguranca` e execute o script atacante:
```bash
python ransoware.py
```
O que acontece agora?  
1. O script será executado e imprimirá: `Ransoware executado! Arquivos criptografados!`
2. Se você olhar a pasta `lab_ciberseguranca`, verá dois novos arquivos:
   - `chave.key`: Esta é a chave de criptografia. No mundo real, o atacante a levaria embora, tornando a recuperação impossível sem ela.
   - `LEIA ISSO.txt`: A nota de resgate com as instruções para pagamento.
3. Se você tentar abrir os arquivos dentro da pasta `test_files`, verá que eles estão ilegíveis (criptografados).

**Passo 2: A Recuperação**

O "resgate" foi pago e o atacante "devolveu" a chave (o arquivo `chave.key` que já está na pasta). Agora, vamos reverter o dano.  

Execute o script de descriptografia:
```bash
python descriptografa.py
```

O que acontece agora?  
1. O script será executado e imprimirá: `Arquivos restaurados com sucesso!`
2. Se você abrir os arquivos dentro de `test_files`, verá que `dados_confidenciais.txt` e `senhas.txt` voltaram ao normal e estão legíveis novamente.
---

## 4. Simulação 2: Keylogger

Um Keylogger é um tipo de spyware que registra secretamente as teclas digitadas pelo usuário. O objetivo é capturar informações sensíveis, como senhas, números de cartão de crédito e conversas privadas.

### 4.1. Cenário A: Keylogger Local (`keylogger.py`)

Esta é a versão mais simples, que salva tudo o que é digitado em um arquivo de texto no próprio computador.

#### Executando a Simulação

1. No terminal, execute o script:  
   `python keylogger.py`
2. O terminal ficará "travado", indicando que o script está em execução e ouvindo suas teclas.
3. Vá para qualquer outro programa (bloco de notas, navegador) e comece a digitar. Por exemplo:  
   `Meu nome é Fulano e minha senha é 123456.`
4. Volte ao terminal e pressione `Ctrl+C` para parar o script.
5. Na mesma pasta, um novo arquivo chamado `log.txt` terá sido criado. Abra-o.
6. Resultado: O arquivo `log.txt` conterá exatamente o que você digitou, por exemplo:  
   `Meu nome é Fulano e minha senha é \n123456.`

### 4.2. Cenário B: Keylogger Remoto por E-mail (`keylogger_email.py`)

Esta versão é mais avançada. Ela não salva os dados localmente. Em vez disso, armazena as teclas na memória e, periodicamente (neste script, a cada 60 segundos), envia os dados capturados por e-mail para o atacante.

#### Configuração Essencial: Gmail e "Senhas de App"

O Google não permite mais que scripts façam login usando apenas seu e-mail e senha (o que era chamado de "Permitir aplicativos menos seguros"). A maneira correta e segura de fazer isso é usando "Senhas de App".

1. Ative a Verificação em Duas Etapas: Você não pode usar Senhas de App sem ela.  
   Vá para sua Conta do Google -> Segurança -> Verificação em duas etapas e ative-a.
2. Gere uma Senha de App:  
   - Na mesma página de Segurança, vá para Senhas de app.  
   - Em "Selecionar app", escolha "Outro (nome personalizado)".  
   - Dê um nome, por exemplo: `Script Python Keylogger`.  
   - O Google irá gerar uma senha de 16 dígitos.
3. Configure o Script:  
   - Abra o arquivo `keylogger_email.py`.  
   - Altere as variáveis no topo:  
     - `EMAIL_ORIGEM`: Coloque o seu endereço do Gmail.  
     - `EMAIL_DESTINO`: Coloque o e-mail que receberá os logs (pode ser o mesmo).  
     - `SENHA_EMAIL`: Coloque a senha de 16 dígitos que o Google gerou, NÃO a sua senha normal do Gmail.

#### Executando a Simulação

1. Após configurar o script, execute-o:  
   `python keylogger_email.py`
2. O script começará a rodar em segundo plano. Digite algumas coisas em outros programas.
3. Aguarde 60 segundos.
4. Verifique a caixa de entrada do `EMAIL_DESTINO`. Você receberá um e-mail com o assunto "Dados capturados pelo keylogger" contendo tudo o que você digitou.

### 4.3. Tornando o Keylogger Furtivo (Técnica `.pyw`)

Em um cenário real, um atacante não pediria para a vítima abrir um terminal e digitar `python keylogger.py`. Eles precisam que o script rode de forma invisível.

No Windows, isso é facilmente alcançado alterando a extensão do arquivo de `.py` para `.pyw`.

- `script.py`: Um script Python normal. Quando executado, uma janela de terminal (console) preta aparece.
- `script.pyw`: Um script "Windowless" (sem janela). O Windows o executa usando o interpretador `pythonw.exe`, que não abre nenhuma janela de terminal.

Se você renomear `keylogger_email.py` para `keylogger_email.pyw` e der dois cliques, o script será executado em segundo plano, invisível para o usuário comum (embora ainda visível no Gerenciador de Tarefas). Esta é uma tática de engenharia social comum para disfarçar malware.

---

## 5. 🛡️ Reflexão sobre Defesa (Blue Team)

Entender como esses ataques funcionam é a melhor maneira de saber como se defender.

### 5.1. Como se Defender de Ransomware

1. **Backup!**  
   Esta é a defesa número 1 e mais eficaz. Se você tem cópias seguras e offline dos seus arquivos, um ataque de ransomware se torna um inconveniente, e não um desastre.  
   Siga a regra 3-2-1: 3 cópias dos seus dados, em 2 tipos de mídia diferentes, com 1 cópia mantida offline (fora do local ou na nuvem com versionamento).

2. **Atualizações de Software (Patching)**  
   Mantenha seu sistema operacional e todos os seus softwares (especialmente navegador e antivírus) atualizados. Muitos ransomwares se espalham explorando vulnerabilidades conhecidas.

3. **Conscientização e "Firewall Humano"**  
   A maioria dos ransomwares entra por e-mails de phishing. Ensine os usuários a nunca abrir anexos suspeitos (`.exe`, `.scr`, `.zip` com ou sem senhas) ou clicar em links estranhos.

4. **Antivírus de Próxima Geração (NGAV)**  
   Soluções modernas de antivírus usam análise heurística e comportamental. Elas podem detectar um script (como o nosso) que começa a ler e criptografar muitos arquivos em sequência e bloqueá-lo antes que ele cause muitos danos.

### 5.2. Como se Defender de Keyloggers

1. **Antivírus e Antimalware**  
   Um bom software de segurança detecta a maioria dos keyloggers conhecidos por suas "assinaturas" ou por seu comportamento suspeito (como "ouvir" teclas).

2. **Firewall de Saída (Outbound)**  
   O `keylogger_email.py` seria inútil se não pudesse enviar os dados. Um firewall bem configurado (como o do próprio Windows) pode ser configurado para bloquear aplicativos desconhecidos de tentarem se conectar à internet.

3. **Gerenciadores de Senhas**  
   Uma defesa excelente e subestimada. Quando você usa um gerenciador de senhas (como Bitwarden, 1Password, etc.), ele preenche automaticamente os campos de login. Você não digita sua senha, portanto, o keylogger não captura nada.

4. **Teclados Virtuais**  
   Para informações ultra-sensíveis (como senhas de banco), muitos sites oferecem um teclado virtual na tela para você clicar com o mouse, quebrando a lógica de captura de teclas.

5. **Sandboxing**  
   Se você receber um arquivo suspeito, execute-o em um ambiente de "sandbox" (como o Windows Sandbox ou uma Máquina Virtual) para ver o que ele faz sem infectar sua máquina real.



