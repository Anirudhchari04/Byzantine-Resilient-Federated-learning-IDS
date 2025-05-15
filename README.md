# 🛡️ Byzantine-Resistant Federated Learning for Intrusion Detection (NSL-KDD)

This project simulates a **Byzantine-resilient Federated Learning (FL)** framework for building robust **Intrusion Detection Systems (IDS)** using the NSL-KDD dataset. It incorporates **malicious (Byzantine) clients** that send poisoned model updates and uses the **Krum aggregation algorithm** to defend against such attacks.

---

## 🚀 Highlights

- ⚙️ Federated Learning with multiple simulated clients.
- 🔐 Byzantine fault tolerance using **Krum Aggregation**.
- 🧠 Local training using PyTorch neural networks.
- 📊 Real-time evaluation across global training rounds.
- 🌐 Scalable design for potential **real-time IDS deployment**.

---

## 📥 Dataset: NSL-KDD
A curated version of the KDD Cup '99 dataset for IDS.
It contains labeled network traffic: normal, dos, probe, r2l, and u2r.

### Dataset Preprocessing:
- One-hot encoding for categorical features
- Standardization of continuous features  
- Label encoding for target classes
## 🔧 Configurable Parameters
```python
NUM_CLIENTS = 5              # Total number of simulated clients
BYZANTINE_CLIENTS = 1        # Number of adversarial (malicious) clients
GLOBAL_ROUNDS = 10           # Number of communication rounds
EPOCHS = 5                   # Number of local training epochs
BATCH_SIZE = 32
LEARNING_RATE = 0.001
```


# 🧠 How It Works – Step by Step

## 1. 📊 Data Preprocessing
- Load the NSL-KDD dataset  
- Clean and preprocess the data:
  - Remove non-numeric/unneeded fields
  - Encode categorical variables (e.g., `protocol_type`, `flag`)
  - Map fine-grained attack labels into broad categories
  - Normalize all feature values

## 2. 🖥️ Federated Learning Setup
- Preprocessed training data is split across multiple clients
- Each client has access only to their local dataset (simulating real-world IDS data distribution)

## 3. ⚠️ Byzantine Client Simulation
Some clients are labeled Byzantine (malicious) and:
- Flip model weights (sign-flip attack)
- Send constant/random weights to poison the global model

## 4. 🏋️ Local Model Training
- Non-Byzantine clients train their model on local data for `EPOCHS`
- Byzantine clients skip training and poison the model update

## 5. 🛡️ Robust Aggregation (Krum)
Byzantine-resilient aggregation algorithm that:
1. Calculates distances between all model updates
2. Selects the update closest to most others (excluding outliers)

## 6. 🌐 Global Model Update
- The selected client model (via Krum) becomes the new global model
- All clients receive this updated model for next round

## 7. 📈 Evaluation
- Global model evaluated on hold-out test set after each round
- Test accuracy reported for each communication round

# 🌍 Real-World IDS Integration

## ✅ Scenario: Distributed Network Security
Each client represents different data sources:
- Firewall at remote branch
- Router at ISP hub  
- Endpoint protection system

## 🔐 Why Federated Learning?
- **Privacy-preserving**: Only model updates shared, not raw traffic logs
- **Bandwidth efficient**: No massive packet data transfers
- **Real-time capable**: Periodic local training + off-peak aggregation

## 🛡️ Robust to Compromise
- Compromised clients can't poison global model (thanks to Krum)
- Enhances resilience in production IDS systems

# 🧰 Production Setup Outline

**Local FL Clients** (deployed as agents):
- Collect/process real-time traffic
- Periodic local training
- Send weight updates to central server

**Central Server**:
- Runs Krum aggregation  
- Broadcasts updated global IDS model

# 🧩 Extension Opportunities
- Add more attack types and traffic logs
- Real-time data ingestion (Kafka/sockets)
- Integration with Suricata/Snort/Zeek
- Alternative robust aggregators:
  - Median
  - Trimmed Mean  
  - Bulyan

# 🧪 Example Output
```arduino
--- Federated Round 1 ---
Client 1: Byzantine – model poisoned
Client 2: Local training complete  
Client 3: Local training complete
Client 4: Local training complete
Client 5: Local training complete
Krum selected model from Client 3
Test Accuracy: 86.75%
```

#📚 References
-NSL-KDD Dataset

-Krum: Byzantine-Robust Aggregation

-Federated Learning – Google AI Blog





