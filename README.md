
# 📁 Nefser

**Nefser** é uma ferramenta de transferência de arquivos via CLI, que permite transferência rápida de arquivos pela rede com apenas uma linha de comando.

---

## 🚀 Como Usar

```bash
python3 nefser.py [argumento] [valor]
python3 nefser.py --help       # Exibir o menu de ajuda
```

---

## ⚙️ Opções

| Opção                | Descrição                                                    |
|----------------------|--------------------------------------------------------------|
| `--help`             | Exibe o menu de ajuda                                        |
| `-recv [arquivo/caminho]`  | Inicia um servidor TCP para receber um arquivo         |
| `-send [arquivo/caminho]`  | Inicia um cliente TCP para enviar um arquivo           |
| `-i [IP/FQDN]`       | Define o endereço IP ou FQDN para envio/recebimento de dados |
| `-p [porta]`         | Define a porta de rede a ser utilizada                       |
| `-t [segundos]`      | Define o tempo de escuta (em segundos) para o recebimento de dados |

---

## 📝 Comportamento Padrão

Se nenhum argumento for fornecido, o comando padrão será:

```bash
python3 nefser.py -recv nefser.bytes -i 0.0.0.0 -p 8080 -t 10
```
Isso executa o Nefser no modo servidor, escutando conexões em todas as interfaces de rede na porta 8080, e com limite de escuta de 10 segundos. Ele aguardará por conexões durante esse tempo, caso não receba nenhum byte, o socket TCP será fechado.

---

## 🔒 Observações

- Certifique-se de que o lado receptor está escutando antes de enviar o arquivo.
- Funciona em redes locais (LAN) e pela internet, se o redirecionamento de portas estiver configurado corretamente.

---

## 📡 Como funciona

Este script segue um processo de 6 etapas para enviar ou receber dados entre um **Remetente** e um **Receptor**.

### 📨 Fluxo do Remetente

1. **Estabelece conexão** — Realiza o Three Way Handshake.  
2. **Envia tamanho do arquivo** — Transmite 5 bytes indicando o tamanho do arquivo.  
3. **Envia hash** — Transmite 33 bytes contendo a hash MD5 do arquivo seguido por um caractere de quebra de linha (`\n`).  
4. **Envia arquivo** — Envia os dados reais do arquivo.  
5. **Validação MD5** — Aguarda a resposta de validação da hash pelo receptor (se aplicável).  
6. **Fecha conexão** — Fecha a conexão.

### 📥 Fluxo do Receptor

1. **Estabelece conexão** — Realiza o handshake de três vias.  
2. **Recebe tamanho do arquivo** — Lê 5 bytes do tamanho do arquivo.  
3. **Recebe hash** — Lê 33 bytes contendo o hash MD5 e o caractere de quebra de linha (`\n`).  
4. **Recebe arquivo** — Recebe os dados reais do arquivo.  
5. **Validação MD5** — Envia o resultado da validação da hash de volta ao remetente (se aplicável).  
6. **Fecha conexão** — Fecha a conexão.

---

Feito por NemarteGH
