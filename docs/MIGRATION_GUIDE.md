# Helix Collective Migration Guide

This guide helps you migrate from the old fragmented repository structure to the new consolidated Helix Collective ecosystem.

## Overview

The Helix Collective has been consolidated from 33 repositories into 17 focused repositories. This consolidation improves maintainability, reduces duplication, and provides a clearer path for community adoption.

### What Changed?

- **5 overlapping repositories** merged into core repositories
- **2 empty stub repositories** deleted
- **3 hub repositories** consolidated into helix-ecosystem-website
- **3 old/unrelated repositories** removed
- **All functionality preserved** in new modular structure

## Migration Timeline

| Date | Action |
|------|--------|
| March 31, 2026 | Consolidation complete, old repos deprecated |
| April 30, 2026 | Old repos archived (read-only) |
| May 31, 2026 | Old repos deleted |

## Import Path Updates

### unified-llm → helix-core

**Old Code**:
```python
from unified_llm import unified_llm
from unified_llm.providers import OpenAIProvider, AnthropicProvider
```

**New Code**:
```python
from helix_core.llm_providers.unified_gateway import unified_llm
from helix_core.llm_providers import OpenAIProvider, AnthropicProvider
```

**Migration Steps**:
1. Update all imports to use `helix_core.llm_providers`
2. Update any configuration files referencing unified-llm
3. Test with your existing code
4. Run tests to ensure functionality

**Example Migration**:
```python
# Old
from unified_llm import LLMGateway

gateway = LLMGateway()
response = gateway.generate(prompt="Hello", provider="openai")

# New
from helix_core.llm_providers.unified_gateway import LLMGateway

gateway = LLMGateway()
response = gateway.generate(prompt="Hello", provider="openai")
```

### agent-consensus → helix-agent-swarm

**Old Code**:
```python
from agent_consensus import AgentCoordinator
from agent_consensus.consensus import ConsensusEngine
```

**New Code**:
```python
from helix_agent_swarm.consensus.coordinator import AgentCoordinator
from helix_agent_swarm.consensus.engine import ConsensusEngine
```

**Migration Steps**:
1. Update imports to use `helix_agent_swarm.consensus`
2. Update any agent initialization code
3. Test multi-agent coordination
4. Verify consensus mechanisms still work

**Example Migration**:
```python
# Old
from agent_consensus import create_agents, coordinate

agents = create_agents(count=3)
result = coordinate(agents, task="analyze_data")

# New
from helix_agent_swarm.consensus import create_agents, coordinate

agents = create_agents(count=3)
result = coordinate(agents, task="analyze_data")
```

### neural-mesh → helix-agent-swarm

**Old Code**:
```python
from neural_mesh import NeuralMesh
from neural_mesh.topology import MeshTopology
```

**New Code**:
```python
from helix_agent_swarm.mesh.neural_mesh import NeuralMesh
from helix_agent_swarm.mesh.topology import MeshTopology
```

**Migration Steps**:
1. Update imports to use `helix_agent_swarm.mesh`
2. Update mesh configuration if needed
3. Test neural mesh connectivity
4. Verify agent communication

**Example Migration**:
```python
# Old
from neural_mesh import create_mesh

mesh = create_mesh(agents=agents, topology="ring")
mesh.connect()

# New
from helix_agent_swarm.mesh import create_mesh

mesh = create_mesh(agents=agents, topology="ring")
mesh.connect()
```

### policy-engine → helix-agent-ethics

**Old Code**:
```python
from policy_engine import PolicyEngine
from policy_engine.rules import Rule, RuleSet
```

**New Code**:
```python
from helix_agent_ethics.policy.engine import PolicyEngine
from helix_agent_ethics.policy.rules import Rule, RuleSet
```

**Migration Steps**:
1. Update imports to use `helix_agent_ethics.policy`
2. Update policy configuration files
3. Test policy enforcement
4. Verify ethical constraints

**Example Migration**:
```python
# Old
from policy_engine import PolicyEngine

engine = PolicyEngine()
engine.load_rules("policies.yaml")

# New
from helix_agent_ethics.policy.engine import PolicyEngine

engine = PolicyEngine()
engine.load_rules("policies.yaml")
```

### helix-chat-engine → helix-web-os

**Old Code**:
```python
from helix_chat_engine import ChatEngine
from helix_chat_engine.models import ChatMessage
```

**New Code**:
```python
from helix_web_os.chat import ChatEngine
from helix_web_os.chat.models import ChatMessage
```

**Migration Steps**:
1. Update imports to use `helix_web_os.chat`
2. Update chat configuration
3. Test chat functionality
4. Verify message handling

**Example Migration**:
```python
# Old
from helix_chat_engine import create_chat_session

session = create_chat_session(user_id="user123")
response = session.send_message("Hello")

# New
from helix_web_os.chat import create_chat_session

session = create_chat_session(user_id="user123")
response = session.send_message("Hello")
```

## Dependency Updates

### Update requirements.txt

**Old**:
```
unified-llm==1.0.0
agent-consensus==1.0.0
neural-mesh==1.0.0
policy-engine==1.0.0
helix-chat-engine==1.0.0
```

**New**:
```
helix-core>=1.0.0
helix-agent-swarm>=1.0.0
helix-agent-ethics>=1.0.0
helix-web-os>=1.0.0
```

### Update setup.py

```python
# Old
install_requires=[
    'unified-llm>=1.0.0',
    'agent-consensus>=1.0.0',
    'neural-mesh>=1.0.0',
    'policy-engine>=1.0.0',
]

# New
install_requires=[
    'helix-core>=1.0.0',
    'helix-agent-swarm>=1.0.0',
    'helix-agent-ethics>=1.0.0',
]
```

## Configuration File Updates

### Update Environment Variables

**Old**:
```bash
export UNIFIED_LLM_API_KEY=xxx
export AGENT_CONSENSUS_TIMEOUT=30
export NEURAL_MESH_TOPOLOGY=ring
export POLICY_ENGINE_CONFIG=policies.yaml
```

**New**:
```bash
export HELIX_CORE_LLM_API_KEY=xxx
export HELIX_AGENT_SWARM_TIMEOUT=30
export HELIX_AGENT_SWARM_MESH_TOPOLOGY=ring
export HELIX_AGENT_ETHICS_POLICY_CONFIG=policies.yaml
```

### Update Configuration Files

**Old (config.yaml)**:
```yaml
unified_llm:
  provider: openai
  model: gpt-4

agent_consensus:
  agents: 3
  timeout: 30

neural_mesh:
  topology: ring
```

**New (config.yaml)**:
```yaml
helix_core:
  llm:
    provider: openai
    model: gpt-4

helix_agent_swarm:
  consensus:
    agents: 3
    timeout: 30
  mesh:
    topology: ring
```

## Code Migration Examples

### Example 1: Simple LLM Usage

**Old Code**:
```python
from unified_llm import LLMGateway

def generate_response(prompt):
    gateway = LLMGateway(provider="openai")
    response = gateway.generate(
        prompt=prompt,
        temperature=0.7,
        max_tokens=500
    )
    return response
```

**New Code**:
```python
from helix_core.llm_providers.unified_gateway import LLMGateway

def generate_response(prompt):
    gateway = LLMGateway(provider="openai")
    response = gateway.generate(
        prompt=prompt,
        temperature=0.7,
        max_tokens=500
    )
    return response
```

**Changes**: Only the import path changed; functionality is identical.

### Example 2: Multi-Agent Coordination

**Old Code**:
```python
from agent_consensus import AgentCoordinator
from neural_mesh import NeuralMesh

coordinator = AgentCoordinator(num_agents=3)
mesh = NeuralMesh(agents=coordinator.agents)

result = coordinator.execute_task(
    task="analyze_data",
    consensus_threshold=0.8
)
```

**New Code**:
```python
from helix_agent_swarm.consensus import AgentCoordinator
from helix_agent_swarm.mesh import NeuralMesh

coordinator = AgentCoordinator(num_agents=3)
mesh = NeuralMesh(agents=coordinator.agents)

result = coordinator.execute_task(
    task="analyze_data",
    consensus_threshold=0.8
)
```

**Changes**: Import paths updated; logic remains the same.

### Example 3: Policy-Aware System

**Old Code**:
```python
from unified_llm import LLMGateway
from policy_engine import PolicyEngine

llm = LLMGateway()
policy = PolicyEngine()

policy.load_rules("policies.yaml")

response = llm.generate(
    prompt="Do something",
    policy_engine=policy
)
```

**New Code**:
```python
from helix_core.llm_providers.unified_gateway import LLMGateway
from helix_agent_ethics.policy.engine import PolicyEngine

llm = LLMGateway()
policy = PolicyEngine()

policy.load_rules("policies.yaml")

response = llm.generate(
    prompt="Do something",
    policy_engine=policy
)
```

**Changes**: Import paths updated; integration pattern unchanged.

## Automated Migration Script

We provide a migration script to help update your codebase:

```bash
# Download the migration script
wget https://github.com/Deathcharge/helix-ecosystem-website/raw/main/tools/migrate.py

# Run the migration
python migrate.py --source-dir /path/to/your/code --dry-run

# Apply the migration
python migrate.py --source-dir /path/to/your/code --apply
```

### What the Script Does

- Scans Python files for old imports
- Generates a report of changes needed
- Optionally applies changes automatically
- Creates backups before modifying files
- Provides detailed migration report

## Testing Your Migration

### 1. Unit Tests

```bash
# Run your existing unit tests
pytest tests/ -v

# Check for any import errors
python -c "from your_module import *"
```

### 2. Integration Tests

```bash
# Test with new imports
pytest tests/integration/ -v

# Test with multiple Helix modules
pytest tests/helix_integration/ -v
```

### 3. Manual Testing

```python
# Test basic functionality
from helix_core.llm_providers.unified_gateway import LLMGateway

gateway = LLMGateway()
response = gateway.generate(prompt="Test")
print(response)
```

## Troubleshooting Migration

### Issue: ModuleNotFoundError

**Problem**: `ModuleNotFoundError: No module named 'unified_llm'`

**Solution**:
1. Verify you've updated all imports
2. Reinstall helix-core: `pip install --upgrade helix-core`
3. Check that old packages are uninstalled: `pip uninstall unified-llm`

### Issue: AttributeError

**Problem**: `AttributeError: module 'helix_core' has no attribute 'LLMGateway'`

**Solution**:
1. Check the correct import path: `from helix_core.llm_providers.unified_gateway import LLMGateway`
2. Verify helix-core is properly installed
3. Check for typos in import statements

### Issue: Configuration Not Found

**Problem**: Configuration files not found after migration

**Solution**:
1. Update configuration file paths in your code
2. Update environment variable names
3. Check that config files are in the correct location

### Issue: Dependency Conflicts

**Problem**: Dependency conflicts when installing new packages

**Solution**:
```bash
# Create a fresh virtual environment
python -m venv venv_new
source venv_new/bin/activate

# Install new dependencies
pip install helix-core helix-agent-swarm helix-agent-ethics

# Migrate your code
# Test thoroughly
```

## Verification Checklist

- [ ] All imports updated to new paths
- [ ] requirements.txt updated
- [ ] setup.py updated
- [ ] Configuration files updated
- [ ] Environment variables updated
- [ ] Unit tests passing
- [ ] Integration tests passing
- [ ] Manual testing completed
- [ ] Old packages uninstalled
- [ ] Documentation updated

## Getting Help

If you encounter issues during migration:

1. **Check Documentation**: Review [ECOSYSTEM_OVERVIEW.md](ECOSYSTEM_OVERVIEW.md)
2. **Search Issues**: Look for similar problems on GitHub
3. **Open an Issue**: Create a detailed issue with:
   - Your old code
   - Your new code
   - Error message
   - Steps to reproduce
4. **Join Community**: Ask in GitHub Discussions
5. **Email Support**: Contact support@helixcollective.dev

## Next Steps

After successful migration:

1. **Update Documentation**: Document any custom changes
2. **Update CI/CD**: Update build and test pipelines
3. **Deploy**: Deploy migrated code to production
4. **Monitor**: Watch for any issues in production
5. **Contribute**: Share your migration experience with the community

---

**Migration Status**: Ready to migrate  
**Support**: Full support available  
**Timeline**: 60 days until old repos are deleted

**Questions?** See [CONTRIBUTING.md](CONTRIBUTING.md) for how to get help!
