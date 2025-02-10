class EthereumBlockchain:
    def __init__(self):
        self.gas_used = 0

    def log_prediction(self, data):
        time.sleep(random.uniform(0.1, 0.3))
        self.gas_used += random.randint(100000, 200000)
        return random.uniform(0.1, 0.3)

class HyperledgerBlockchain:
    def __init__(self):
        self.energy_consumed = 0

    def log_prediction(self, data):
        time.sleep(random.uniform(0.05, 0.15))
        self.energy_consumed += random.uniform(0.01, 0.05)
        return random.uniform(0.05, 0.15)

class LightweightBlockchain:
    def __init__(self):
        self.energy_consumed = 0

    def log_prediction(self, data):
        time.sleep(random.uniform(0.02, 0.1))
        self.energy_consumed += random.uniform(0.005, 0.02)
        return random.uniform(0.02, 0.1)

def calculate_scalability(blockchain_type):
    if blockchain_type == "Ethereum":
        gas_weight = 0.7
        latency_weight = 0.3
        avg_latency = np.mean([random.uniform(0.1, 0.3) for _ in range(100)])
        avg_gas_used = np.mean([random.randint(100000, 200000) for _ in range(100)])
        scalability = (1 - (gas_weight * (avg_gas_used / 200000))) + (latency_weight * (1 - (avg_latency / 0.3)))
    elif blockchain_type == "Hyperledger":
        energy_weight = 0.6
        latency_weight = 0.4
        avg_latency = np.mean([random.uniform(0.05, 0.15) for _ in range(100)])
        avg_energy = np.mean([random.uniform(0.01, 0.05) for _ in range(100)])
        scalability = (1 - (energy_weight * (avg_energy / 0.05))) + (latency_weight * (1 - (avg_latency / 0.15)))
    elif blockchain_type == "Lightweight":
        energy_weight = 0.5
        latency_weight = 0.5
        avg_latency = np.mean([random.uniform(0.02, 0.1) for _ in range(100)])
        avg_energy = np.mean([random.uniform(0.005, 0.02) for _ in range(100)])
        scalability = (1 - (energy_weight * (avg_energy / 0.02))) + (latency_weight * (1 - (avg_latency / 0.1)))
    else:
        scalability = 1.0  # Default for unknown blockchain type
    return scalability

def calculate_metrics(latencies, tx_count, blockchain_type, energy_consumed=None):
    throughput = tx_count / sum(latencies) if latencies else 0
    avg_latency = np.mean(latencies) if latencies else 0
    scalability = calculate_scalability(blockchain_type)
    metrics = {
        "Throughput": throughput,
        "Avg Latency (s)": avg_latency,
        "Scalability": scalability
    }
    if energy_consumed:
        metrics["Energy (kWh)"] = energy_consumed
    return metrics

# Initialize Blockchain Instances
eth_blockchain = EthereumBlockchain()
hl_blockchain = HyperledgerBlockchain()
lb_blockchain = LightweightBlockchain()

# Sample Predictions
predictions = ["Normal", "Attack"] * 5000

# Simulate Transactions for Metrics
num_transactions = 1000

eth_latencies = [eth_blockchain.log_prediction(str(p).encode('utf-8')) for p in predictions[:num_transactions]]
hl_latencies = [hl_blockchain.log_prediction(str(p).encode('utf-8')) for p in predictions[:num_transactions]]
lb_latencies = [lb_blockchain.log_prediction(str(p).encode('utf-8')) for p in predictions[:num_transactions]]

# Calculate and Print Metrics
eth_metrics = calculate_metrics(eth_latencies, num_transactions, "Ethereum", energy_consumed=eth_blockchain.gas_used * 0.00001)
hl_metrics = calculate_metrics(hl_latencies, num_transactions, "Hyperledger", energy_consumed=hl_blockchain.energy_consumed)
lb_metrics = calculate_metrics(lb_latencies, num_transactions, "Lightweight", energy_consumed=lb_blockchain.energy_consumed)

# Print Results
print("Ethereum Blockchain Metrics:", eth_metrics)
print("Hyperledger Blockchain Metrics:", hl_metrics)
print("Lightweight Blockchain Metrics:", lb_metrics)
