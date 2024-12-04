# CoreHiveAi Blockchain

Uma blockchain descentralizada focada em computação distribuída.

## Características Principais

- Token: CHAI
- Supply Total: 60 milhões
- Dificuldade inicial de mineração: 4
- Sistema de carteiras integrado
- Interface para desenvolvedores
- Rede de mineração distribuída

## Componentes do Sistema

1. **Blockchain Core**
   - Implementação da blockchain
   - Sistema de consenso
   - Gerenciamento de transações

2. **Sistema de Carteiras**
   - Geração de endereços únicos
   - Verificação de saldo
   - Transferências

3. **Minerador**
   - Cliente de mineração
   - Conexão com a rede
   - Recompensas de mineração

4. **Interface do Desenvolvedor**
   - API para submissão de tarefas
   - Acesso ao poder computacional
   - Monitoramento de recursos

## Instalação

### Servidor AWS
```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar dependências
sudo apt install python3 python3-pip git -y

# Clonar repositório
git clone [URL_DO_REPO]

# Instalar requisitos
pip3 install -r requirements.txt

# Iniciar servidor
python3 blockchain/main.py
```

### Windows
1. Instale Python 3.8+ do site oficial
2. Baixe o repositório
3. Execute `pip install -r requirements.txt`
4. Inicie os componentes desejados:
   - Blockchain: `python blockchain/main.py`
   - Minerador: `python blockchain/miner.py`
   - Carteira: `python blockchain/wallet.py`
   - Interface Dev: `python blockchain/dev_interface.py`

## Uso

### Gerando uma Carteira
```python
from blockchain.wallet import Wallet

wallet = Wallet()
address = wallet.generate_address("MeuNome")
print(f"Seu endereço: {address}")
```

### Iniciando Mineração
```python
from blockchain.miner import Miner

miner = Miner("SEU_ENDERECO_WALLET", "http://servidor:5000")
miner.start_mining()
```

### Interface do Desenvolvedor
```python
from blockchain.dev_interface import DeveloperInterface

dev = DeveloperInterface("http://servidor:5000", "sua_chave_api")
task = dev.submit_computation_task({"type": "processing", "data": "..."})
```

## Sugestões de Melhorias Futuras

1. **Segurança**
   - Implementar criptografia avançada
   - Sistema de validação de nós
   - Proteção contra ataques

2. **Escalabilidade**
   - Sharding
   - Otimização de consenso
   - Cache distribuído

3. **Funcionalidades**
   - Contratos inteligentes
   - Sistema de staking
   - DEX integrada
   - Bridges com outras blockchains

4. **Interface**
   - Dashboard web
   - Aplicativo móvel
   - Ferramentas de análise

## Contribuição

Contribuições são bem-vindas! Por favor, leia o guia de contribuição antes de submeter pull requests.