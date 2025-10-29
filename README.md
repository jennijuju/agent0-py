# Agent0 Python SDK

A Python SDK for agent portability, discovery and trust based on ERC-8004.

## Features

- **Agent Identity Management**: Create, register, and manage agent identities on-chain
- **Reputation System**: Give and receive feedback with cryptographic authentication
- **Agent Discovery**: Search and discover agents across chains with semantic search
- **Trust Models**: Support for multiple trust models including reputation and validation
- **MCP/A2A Integration**: Automatic discovery of agent capabilities and endpoints
- **IPFS Integration**: Decentralized storage for agent registration files and feedback

## Installation

### From PyPI

```bash
pip install agent0-sdk
```

### Development Installation

```bash
cd agent0-py
pip install -e .
```

## Quick Start

```python
from agent0_sdk import SDK
import os

# Initialize SDK with IPFS and subgraph (recommended for full functionality)
sdk = SDK(
    chainId=11155111,  # Ethereum Sepolia testnet
    rpcUrl=os.getenv("RPC_URL"),
    signer=os.getenv("PRIVATE_KEY"),
    ipfs="pinata",
    pinataJwt=os.getenv("PINATA_JWT")
    # Subgraph URL auto-defaults from DEFAULT_SUBGRAPH_URLS
)

# Create an agent
agent = sdk.createAgent(
    name="My AI Agent",
    description="An intelligent assistant for various tasks",
    image="https://example.com/agent-image.png"
)

# Configure endpoints
agent.setMCP("https://mcp.example.com/")
agent.setA2A("https://a2a.example.com/agent-card.json")

# Set wallet and trust models
agent.setAgentWallet("0x742d35Cc6634C0532925a3b844Bc9e7595f0bEb", chainId=11155111)
agent.setTrust(reputation=True, cryptoEconomic=True)

# Add metadata and set status
agent.setMetadata({
    "version": "1.0.0",
    "category": "ai-assistant"
})
agent.setActive(True)

# Register on-chain with IPFS
agent.registerIPFS()
print(f"Agent registered with ID: {agent.agentId}")
print(f"Agent URI: {agent.agentURI}")

# Search for agents
results = sdk.searchAgents(
    name="AI",  # substring search
    mcp=True
)
```

## Agent Transfer

Transfer agent ownership to another address. Only the current owner can transfer an agent.

```python
from agent0_sdk import SDK
import os

# Initialize SDK
sdk = SDK(
    chainId=11155111,
    rpcUrl=os.getenv("RPC_URL"),
    signer=os.getenv("PRIVATE_KEY")
)

# Transfer using Agent instance
agent = sdk.loadAgent("11155111:123")
transfer_result = agent.transfer("0x742d35Cc6634C0532925a3b8D4C9db96C4b4d8b6")

# Or transfer using SDK directly
transfer_result = sdk.transferAgent("11155111:123", "0x742d35Cc6634C0532925a3b8D4C9db96C4b4d8b6")

print(f"Transfer transaction: {transfer_result['txHash']}")
print(f"New owner: {transfer_result['to']}")
```

**Important Notes:**
- Only the current owner can transfer the agent
- Agent URI, metadata, and all other data remain unchanged
- Transfer is irreversible - ensure the new owner is correct
- Invalid addresses and self-transfers are automatically rejected

## Smart Contracts

The smart contract ABIs are located at `../smart-contracts/` (relative to this directory).

## Documentation

Full documentation is available at [sdk.ag0.xyz](https://sdk.ag0.xyz).

## Examples

See the `examples/` directory for complete working examples:
- `test_registration.py` - Agent registration with HTTP URI
- `test_registrationIpfs.py` - Agent registration with IPFS
- `test_feedback.py` - Feedback flow
- `test_search.py` - Agent search and discovery
- `test_transfer.py` - Agent ownership transfer

## Development

### Setup

```bash
cd agent0-py
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -e ".[dev]"
```

### Running Tests

```bash
pytest tests/
```

### Running Examples

```bash
# Set up .env file in project root with your configuration
cd examples
python test_registration.py
```

## License

MIT License - see LICENSE file for details.

