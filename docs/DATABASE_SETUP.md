# Guia de Instalação do Banco de Dados - CoreHiveAi

## 1. Instalação no Servidor AWS

### Pré-requisitos
```bash
# Atualizar o sistema
sudo apt update && sudo apt upgrade -y

# Instalar SQLite e dependências
sudo apt install sqlite3 python3-sqlite3 -y
```

### Configuração do Banco de Dados

1. Conecte-se ao seu servidor AWS:
```bash
ssh -i sua-chave.pem ubuntu@18.212.11.58
```

2. Crie o diretório para o projeto:
```bash
mkdir -p /opt/corehiveai
cd /opt/corehiveai
```

3. Configure as permissões:
```bash
sudo chown -R ubuntu:ubuntu /opt/corehiveai
chmod 755 /opt/corehiveai
```

4. Inicialize o banco de dados:
```bash
cd /opt/corehiveai
sqlite3 corehiveai.db
```

5. Verifique a instalação:
```bash
sqlite3 corehiveai.db "SELECT sqlite_version();"
```

## 2. Configuração do Projeto

1. Clone o repositório:
```bash
git clone [URL_DO_REPO] /opt/corehiveai
cd /opt/corehiveai
```

2. Instale as dependências Python:
```bash
pip3 install -r requirements.txt
```

3. Configure o caminho do banco de dados:
Edite o arquivo `blockchain/config.py` e atualize o caminho do banco:
```python
BLOCKCHAIN_CONFIG = {
    ...
    'DATABASE_PATH': '/opt/corehiveai/corehiveai.db'
    ...
}
```

4. Inicialize o banco de dados:
```bash
python3 -c "
from blockchain.database.manager import DatabaseManager
db = DatabaseManager()
db.initialize_database()
db.close()
"
```

## 3. Verificação da Instalação

1. Teste a conexão com o banco:
```bash
python3 -c "
from blockchain.database.manager import DatabaseManager
db = DatabaseManager()
print('Conexão estabelecida com sucesso!' if db else 'Erro na conexão')
db.close()
"
```

2. Verifique as tabelas criadas:
```bash
sqlite3 /opt/corehiveai/corehiveai.db ".tables"
```

## 4. Manutenção do Banco de Dados

### Backup
```bash
# Criar backup
sqlite3 /opt/corehiveai/corehiveai.db ".backup '/opt/corehiveai/backups/backup.db'"
```

### Restauração
```bash
# Restaurar backup
sqlite3 /opt/corehiveai/corehiveai.db ".restore '/opt/corehiveai/backups/backup.db'"
```

### Monitoramento
```bash
# Verificar tamanho do banco
du -h /opt/corehiveai/corehiveai.db

# Verificar integridade
sqlite3 /opt/corehiveai/corehiveai.db "PRAGMA integrity_check;"
```

## 5. Segurança

1. Configure as permissões corretas:
```bash
sudo chown www-data:www-data /opt/corehiveai/corehiveai.db
sudo chmod 640 /opt/corehiveai/corehiveai.db
```

2. Crie um diretório para backups:
```bash
sudo mkdir -p /opt/corehiveai/backups
sudo chown www-data:www-data /opt/corehiveai/backups
sudo chmod 750 /opt/corehiveai/backups
```

## 6. Troubleshooting

### Problemas Comuns e Soluções

1. Erro de permissão:
```bash
sudo chown -R www-data:www-data /opt/corehiveai
sudo chmod -R 755 /opt/corehiveai
sudo chmod 640 /opt/corehiveai/corehiveai.db
```

2. Banco de dados corrompido:
```bash
# Verificar e reparar
sqlite3 /opt/corehiveai/corehiveai.db "VACUUM;"
```

3. Erro de conexão:
```bash
# Verificar logs
tail -f /var/log/syslog | grep sqlite
```

## 7. Inicialização Automática

Crie um serviço systemd para iniciar automaticamente:

```bash
sudo nano /etc/systemd/system/corehiveai.service
```

Conteúdo:
```ini
[Unit]
Description=CoreHiveAi Blockchain
After=network.target

[Service]
User=www-data
WorkingDirectory=/opt/corehiveai
ExecStart=/usr/bin/python3 blockchain/main.py
Restart=always

[Install]
WantedBy=multi-user.target
```

Ative o serviço:
```bash
sudo systemctl enable corehiveai
sudo systemctl start corehiveai
```