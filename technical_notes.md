# Technical Notes

Existing content of the file...

## Running wallet on testnet

### Prerequisites
- Node.js and npm installed
- Docker and Docker Compose installed (for full environment setup)
- Access to TON testnet faucet for test tokens
- Git installed

### Quick Start for Hot Wallet on Testnet

1. **Clone the wallet repository**:
```console
git clone https://github.com/your-repo/ton-wallet.git
cd ton-wallet
```

2. **Install dependencies**:
```console
npm install
```

3. **Configure environment for testnet**:
Create `.env` file in root directory:
```
NETWORK=testnet
LITESERVER=https://testnet-v4.tonhubapi.com
LITESERVER_KEY=your_liteserver_key
WALLET_TYPE=highload_v2
SEED_PHRASE=your_12_word_seed_phrase
IS_TESTNET=true
```

4. **Generate hot wallet from seed phrase**:
```console
npm run generate:hot-wallet
```

5. **Deploy hot wallet to testnet**:
```console
npm run deploy:hot-wallet
```

6. **Start the wallet service**:
```console
npm start
```

### Deploying Deposit Wallets on Testnet

1. **For each deposit wallet, create separate configuration**:
```
SUBWALLET_ID=unique_id_for_each_deposit
DEPOSIT_ADDRESS=generated_address
```

2. **Generate deposit wallet**:
```console
npm run generate:deposit-wallet -- --subwallet-id=1
```

3. **Deploy deposit wallet**:
```console
npm run deploy:deposit-wallet -- --subwallet-id=1
```

4. **Verify deployment**:
```console
npm run check:balance -- --address=your_deposit_address
```

### Testing Wallet Operations on Testnet

1. **Get testnet TON tokens**:
   - Visit https://testnet-faucet.ton.org
   - Send tokens to your hot wallet address

2. **Test transfer to deposit wallet**:
```console
npm run test:transfer -- --to=deposit_address --amount=1.0
```

3. **Test withdrawal from deposit to hot wallet**:
```console
npm run test:withdrawal -- --from=deposit_address --amount=0.5
```

4. **Check transaction status**:
```console
npm run check:transaction -- --tx-hash=transaction_hash
```

### Docker Compose Setup for Full Testnet Environment

```console
# Start database
docker-compose -f docker-compose-testnet.yml up -d postgres

# Start wallet services
docker-compose -f docker-compose-testnet.yml up -d wallet-hot
docker-compose -f docker-compose-testnet.yml up -d wallet-deposit

# Start monitoring (optional)
docker-compose -f docker-compose-testnet.yml up -d grafana
docker-compose -f docker-compose-testnet.yml up -d prometheus

# View logs
docker-compose -f docker-compose-testnet.yml logs -f wallet-hot
```

### Environment Variables for Testnet

| Variable | Description | Example |
|----------|-------------|---------|
| `NETWORK` | Blockchain network | `testnet` |
| `LITESERVER` | Testnet liteserver URL | `https://testnet-v4.tonhubapi.com` |
| `LITESERVER_KEY` | Liteserver API key | `your_key_here` |
| `IS_TESTNET` | Testnet flag | `true` |
| `WALLET_TYPE` | Type of wallet | `highload_v2` or `v3r2` |
| `SEED_PHRASE` | Wallet seed phrase (12 words) | `word1 word2 ... word12` |
| `DB_HOST` | Database host | `localhost` |
| `DB_PORT` | Database port | `5432` |
| `DB_NAME` | Database name | `wallet_testnet` |

### Troubleshooting Testnet Issues

**Issue: "Invalid seed phrase"**
- Ensure you have exactly 12 words
- Check for typos in the seed phrase
- Regenerate if needed

**Issue: "Insufficient balance for deployment"**
- Verify testnet tokens were received
- Use faucet again at https://testnet-faucet.ton.org
- Wait for transaction confirmation

**Issue: "Liteserver connection timeout"**
- Check your internet connection
- Verify LITESERVER URL is correct
- Try alternative liteserver endpoint

**Issue: "Transaction failed"**
- Check wallet has enough TON for fees
- Verify recipient address is correct
- Check transaction expiration time

### Monitoring Wallet Status on Testnet

Access the API endpoints:
```console
# Check wallet balance
curl http://localhost:8080/v1/wallet/balance

# Get recent transactions
curl http://localhost:8080/v1/wallet/transactions

# Check deployment status
curl http://localhost:8080/v1/wallet/status
```

### Useful Testnet Resources

- **Testnet Explorer**: https://testnet.tonviewer.com
- **Testnet Faucet**: https://testnet-faucet.ton.org
- **Documentation**: https://ton.org/docs
- **Community**: https://t.me/tondev