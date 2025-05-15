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
📥 Dataset: NSL-KDD
A curated version of the KDD Cup '99 dataset for IDS.

It contains labeled network traffic: normal, dos, probe, r2l, and u2r.

The dataset is preprocessed using:

One-hot encoding for categorical features.

Standardization of continuous features.

Label encoding for target classes.

🔧 Configurable Parameters
python
Copy
Edit
NUM_CLIENTS = 5              # Total number of simulated clients
BYZANTINE_CLIENTS = 1        # Number of adversarial (malicious) clients
GLOBAL_ROUNDS = 10           # Number of communication rounds
EPOCHS = 5                   # Number of local training epochs
BATCH_SIZE = 32
LEARNING_RATE = 0.001
🧠 How It Works – Step by Step
1. 📊 Data Preprocessing
Load the NSL-KDD dataset.

Clean and preprocess the data:

Remove non-numeric/unneeded fields.

Encode categorical variables (e.g., protocol_type, flag).

Map fine-grained attack labels into broad categories.

Normalize all feature values.

2. 🖥️ Federated Learning Setup
The preprocessed training data is split across multiple clients.

Each client has access only to their local dataset — simulating real-world scenarios like IDS data on different firewalls, routers, or devices.

3. ⚠️ Byzantine Client Simulation
Some clients are labeled Byzantine (malicious).

Instead of performing valid training, they:

Flip model weights (sign-flip attack).

Send constant/random weights to poison the global model.

This simulates model poisoning or adversarial behavior in real-world federated systems.

4. 🏋️ Local Model Training
Each non-Byzantine client trains its model on its local data for a set number of epochs.

Byzantine clients skip training and poison the model update.

5. 🛡️ Robust Aggregation (Krum)
Krum is a Byzantine-resilient aggregation algorithm.

For each client model update:

It calculates distances to all other updates.

Picks the update that is closest to most others (excluding potential malicious ones).

This reduces the influence of outliers and poisoned updates.

6. 🌐 Global Model Update
The selected client model (via Krum) becomes the new global model.

All clients will use this global model in the next round.

7. 📈 Evaluation
After each communication round, the global model is evaluated using a hold-out test set.

Test accuracy is reported for each round.

🌍 Integration into Real-Time IDS Systems
This federated IDS framework can be adapted for real-world deployment as follows:

✅ Scenario: Distributed Network Security
Each client represents a different data source:

A firewall at a remote branch

A router at an ISP hub

An endpoint protection system on a company device

Each source has local traffic logs and cannot share raw data due to privacy/regulatory issues.

🔐 Why Federated Learning?
Privacy-preserving: Only model updates are shared, not raw traffic logs.

Bandwidth efficient: No need to transfer huge packet datasets.

Real-time capable: Local training can be periodic; global aggregation can occur during low-traffic times.

🛡️ Robust to Compromise
Even if some clients are compromised (e.g., by malware), their poisoned updates can be ignored by robust aggregation (like Krum).

This enhances resilience in production-grade IDS systems.

🧰 Production Setup Outline
Deploy local FL clients as agents on firewalls/routers.

Each agent:

Collects and processes real-time traffic data.

Periodically trains a local model.

Sends weight updates to the central server.

Central server:

Runs robust aggregation (Krum).

Broadcasts the updated global IDS model back to all clients.

🧩 Extending This Project
You can enhance the system by:

Adding more attack types and traffic logs.

Supporting real-time data ingestion (via Kafka or sockets).

Integrating with monitoring tools like Suricata, Snort, or Zeek.

Using other robust aggregators (Median, Trimmed Mean, Bulyan).

🧪 Example Output
arduino
Copy
Edit
--- Federated Round 1 ---
Client 1: Byzantine – model poisoned
Client 2: Local training complete
Client 3: Local training complete
Client 4: Local training complete
Client 5: Local training complete
Krum selected model from Client 3
Test Accuracy: 86.75%
📚 References
NSL-KDD Dataset

Krum: Byzantine-Robust Aggregation

Federated Learning Overview – Google
