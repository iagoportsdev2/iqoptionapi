# IQ Option API - Python Wrapper

[![Python](https://img.shields.io/badge/Python-3.6%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Uma API Python completa e não oficial para trading na plataforma IQ Option. Esta biblioteca permite automatizar operações de trading, obter dados de mercado em tempo real e gerenciar contas através de uma interface programática.

## ⚠️ Aviso Legal

Esta é uma API **não oficial** da IQ Option. Use por sua própria conta e risco. O trading de opções binárias e forex envolve riscos significativos e você pode perder todo o seu capital investido.

## 🚀 Características Principais

### 🔐 Autenticação e Conexão
- Login seguro com email e senha
- Suporte para autenticação de dois fatores (2FA)
- Gerenciamento automático de sessões
- Reconexão automática em caso de falha

### 📊 Opções Binárias
- Compra de opções binárias com diferentes durações
- Obtenção de informações de lucro em tempo real
- Histórico de apostas e resultados
- Múltiplas operações simultâneas

### 💹 Opções Digitais
- Trading de opções digitais spot
- Lista de strikes disponíveis
- Cálculo de lucros em tempo real
- Venda antecipada de posições

### 📈 Dados de Mercado
- Candlesticks históricos e em tempo real
- Múltiplos timeframes (1min a 1 mês)
- Indicadores técnicos
- Humor dos traders (sentimento de mercado)
- Cotações em tempo real

### 💰 Gerenciamento de Conta
- Consulta de saldo atual
- Histórico de transações
- Alteração entre conta real e prática
- Informações do perfil do usuário

### 🔧 Trading Avançado
- Orders pendentes (forex)
- Stop Loss e Take Profit
- Alavancagem disponível
- Posições abertas e fechadas
- CFDs e Forex

## 📦 Instalação

```bash
git clone https://github.com/iagoportsdev2/iqoptionapi.git
cd iqoptionapi
pip install -r requirements.txt
```

## 🔧 Uso Básico

### Conectando à API

```python
from iqoptionapi.stable_api import IQ_Option

# Configurar conexão
email = "seu_email@exemplo.com"
password = "sua_senha"

# Criar instância da API
api = IQ_Option(email, password)

# Conectar (conta prática por padrão)
check, reason = api.connect()

if check:
    print("Conectado com sucesso!")
else:
    print(f"Falha na conexão: {reason}")

# Alterar para conta real (opcional)
api.change_balance("REAL")
```

### Exemplos de Uso

#### 1. Opções Binárias

```python
# Comprar opção binária
money = 10  # Valor da aposta
active = "EURUSD"  # Ativo
direction = "call"  # call ou put
duration = 1  # Duração em minutos

check, buy_id = api.buy(money, active, direction, duration)

if check:
    print(f"Compra realizada! ID: {buy_id}")
    
    # Verificar resultado
    option_info = api.get_betinfo(buy_id)
    print(f"Resultado: {option_info}")
```

#### 2. Obter Candlesticks

```python
import time

# Obter velas históricas
active = "EURUSD"
interval = 60  # 1 minuto
count = 100  # Número de velas
endtime = time.time()

candles = api.get_candles(active, interval, count, endtime)

for candle in candles:
    print(f"Tempo: {candle['from']}, Open: {candle['open']}, Close: {candle['close']}")
```

#### 3. Velas em Tempo Real

```python
# Iniciar stream de velas
api.start_candles_one_stream("EURUSD", 60)

# Obter velas em tempo real
while True:
    candles = api.get_realtime_candles("EURUSD", 60)
    if candles:
        latest = candles[-1]
        print(f"Última vela: {latest}")
    time.sleep(1)
```

#### 4. Opções Digitais

```python
# Obter lista de strikes
active = "EURUSD"
duration = 60  # segundos

strikes = api.get_realtime_strike_list(active, duration)
print(f"Strikes disponíveis: {strikes}")

# Comprar opção digital
amount = 10
instrument_id = "exemplo_id_do_instrumento"

check, buy_info = api.buy_digital(amount, instrument_id)
if check:
    print(f"Opção digital comprada: {buy_info}")
```

#### 5. Informações da Conta

```python
# Obter saldo
balance = api.get_balance()
print(f"Saldo atual: {balance}")

# Obter perfil
profile = api.get_profile_ansyc()
print(f"Informações do perfil: {profile}")

# Listar todos os saldos
all_balances = api.get_balances()
print(f"Todos os saldos: {all_balances}")
```

## 📚 Principais Métodos da API

### Conexão e Autenticação
- `connect()` - Conectar à plataforma
- `connect_2fa(sms_code)` - Conectar com 2FA
- `send_sms_code()` - Enviar código SMS para 2FA

### Opções Binárias
- `buy(price, active, direction, duration)` - Comprar opção binária
- `buy_multi()` - Múltiplas compras simultâneas
- `get_betinfo(bet_id)` - Informações da aposta
- `get_optioninfo(limit)` - Histórico de opções

### Opções Digitais
- `buy_digital_spot(active, amount, action, duration)` - Comprar opção digital
- `get_digital_current_profit(active, duration)` - Lucro atual
- `get_realtime_strike_list(active, duration)` - Lista de strikes
- `sell_digital_option(position_id)` - Vender posição

### Dados de Mercado
- `get_candles(active, interval, count, endtime)` - Velas históricas
- `get_realtime_candles(active, size)` - Velas em tempo real
- `start_candles_one_stream(active, size)` - Iniciar stream de velas
- `get_traders_mood(active)` - Humor dos traders
- `get_technical_indicators(active)` - Indicadores técnicos

### Conta e Perfil
- `get_balance()` - Saldo atual
- `get_balances()` - Todos os saldos
- `change_balance(mode)` - Alterar tipo de conta ("PRACTICE"/"REAL")
- `get_profile_ansyc()` - Informações do perfil
- `get_currency()` - Moeda da conta

### Trading Avançado (Forex/CFD)
- `buy_order()` - Ordem de forex/CFD
- `get_positions()` - Posições abertas
- `close_position(position_id)` - Fechar posição
- `get_available_leverages(active)` - Alavancagens disponíveis

## 🔤 Códigos de Ativos

A API trabalha com códigos específicos para cada ativo:

### Principais Pares de Moedas
- `EURUSD` - Euro/Dólar Americano
- `GBPUSD` - Libra/Dólar Americano
- `USDJPY` - Dólar Americano/Iene Japonês
- `AUDUSD` - Dólar Australiano/Dólar Americano
- `USDCAD` - Dólar Americano/Dólar Canadense

### Criptomoedas
- `BTCUSD` - Bitcoin/Dólar
- `ETHUSD` - Ethereum/Dólar
- `LTCUSD` - Litecoin/Dólar

Use `api.get_all_ACTIVES_OPCODE()` para obter a lista completa de ativos disponíveis.

## ⚙️ Configurações Avançadas

### Timeframes Suportados
```python
# Timeframes disponíveis (em segundos)
timeframes = [1, 5, 10, 15, 30, 60, 120, 300, 600, 900, 1800, 3600, 7200, 14400, 28800, 43200, 86400, 604800, 2592000]
```

### Gerenciamento de Erros
```python
try:
    api.connect()
    # Suas operações aqui
except Exception as e:
    print(f"Erro: {e}")
    # Reconectar se necessário
    api.re_subscribe_stream()
```

## 📊 Estrutura de Dados

### Candlestick
```python
{
    'from': timestamp,      # Timestamp de abertura
    'to': timestamp,        # Timestamp de fechamento  
    'open': float,          # Preço de abertura
    'close': float,         # Preço de fechamento
    'high': float,          # Preço máximo
    'low': float,           # Preço mínimo
    'volume': int           # Volume
}
```

### Resultado de Aposta
```python
{
    'id': int,              # ID da aposta
    'active': str,          # Ativo
    'amount': float,        # Valor apostado
    'direction': str,       # call/put
    'result': str,          # win/loss
    'profit': float         # Lucro/prejuízo
}
```

## 🛠️ Desenvolvimento

### Estrutura do Projeto
```
iqoptionapi/
├── __init__.py           # Inicialização do módulo
├── api.py               # Classe principal da API
├── stable_api.py        # API estável com métodos simplificados
├── constants.py         # Constantes e códigos
├── http/               # Módulos HTTP
│   ├── login.py        # Autenticação
│   ├── auth.py         # Autorização
│   └── ...
├── ws/                 # WebSocket
│   ├── client.py       # Cliente WebSocket
│   ├── chanels/        # Canais de comunicação
│   └── objects/        # Objetos de dados
└── README.md           # Este arquivo
```

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/nova-feature`)
3. Commit suas mudanças (`git commit -am 'Adiciona nova feature'`)
4. Push para a branch (`git push origin feature/nova-feature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

## ⚠️ Disclaimer

- Esta API não é oficialmente suportada pela IQ Option
- Use apenas para fins educacionais e de pesquisa
- Trading envolve riscos financeiros significativos
- Os desenvolvedores não se responsabilizam por perdas financeiras
- Sempre teste em conta demo antes de usar dinheiro real

## 📞 Suporte

Para problemas, bugs ou sugestões:
1. Abra uma [Issue](https://github.com/iagoportsdev2/iqoptionapi/issues)
2. Descreva o problema detalhadamente
3. Inclua código de exemplo se possível

---

**Desenvolvido com ❤️ para a comunidade de trading algorítmico**