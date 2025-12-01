
# Enabling Azure Voice Live API with BYOM
 ![flow](/Images/ByomHeader.png)
# A Complete Technical Guide
### *Fine-tuned accuracy, regional compliance, and secure real-time voice intelligence*

Azure **Voice Live API**, combined with **Bring Your Own Model (BYOM)**, enables you to integrate your own custom or fine‑tuned models into real‑time voice workflows. This is ideal for scenarios where enterprises require:

- **Fine‑tuned or custom LLMs**
- **Provisioned throughput (PTUs)** for consistent, predictable latency
- **Custom content safety rules**
- **Regional model deployment** (example: India-only) for compliance  

BYOM allows you to keep full control of the *intelligence* while Voice Live delivers *ultra‑low‑latency voice interactions*.




# Why BYOM for Voice Live?

- Use **fine‑tuned Azure OpenAI or Foundry models**
- Zero secret leakage using **Managed Identity**
- Stay region-compliant by avoiding globally deployed models
- Predictable performance using **PTUs**
- Apply enterprise-specific **content moderation pipelines**



## Managed Models vs BYOM

| Feature | Managed Models (Default) | BYOM (Bring Your Own Model) |
|--|||
| **Control** | Limited | Full control |
| **Compliance** | Region-dependent | Guaranteed data residency |
| **Performance** | Shared capacity | Dedicated PTUs |
| **Customization** | Minimal | High – fine-tuned or custom |
| **Cost** | Pay-per-use | PTU-based flexibility |


# BYOM Integration Modes

| BYOM Mode | Description | Example Models |
|-|-|-|
| **byom-azure-openai-realtime** | Real‑time models for streaming voice interactions | `gpt-realtime`, `gpt-realtime-mini` |
| **byom-azure-openai-chat-completion** | Chat/text completion models, including Foundry models | `gpt-4.1`, `gpt-5-chat`, `grok-3`, `gpt-4o` |




## Architecture Overview

 ![flow](/Images/byom.png)

This architecture enables:

- Low-latency, real-time conversations  
- Secure access using Managed Identity  
- Decoupled deployment of voice + model  
- Cross-region model flexibility  



# Implementation Guide

Below is the complete flow to enable Voice Live API using your own model (BYOM).



## Step 1: Create Voice Live Resource

1. Open **Azure Portal**
2. Create a **Voice Live** resource (Azure AI Foundry)
3. Select the region for real-time voice processing



## Step 2: Deploy Your Model (GPT-4o or Fine-Tuned Model)

1. Create/Use a separate Azure AI Foundry resource  
2. Deploy:  
   - gpt-4o  
   - gpt-4.1
   - gpt-5-chat
   - grok-3
   - gpt-realtime
   - gpt-realtime-mini
   - A fine-tuned model  
3. Ensure the deployment supports **chat completion + voice**

Reference:  
[Deploy Foundry Models](https://learn.microsoft.com/en-us/azure/ai-foundry/foundry-models/how-to/deploy-foundry-models)



## Step 3: Authentication Setup

### Microsoft Entra ID Authentication

When using Microsoft Entra ID authentication with Voice Live API, you need to configure proper permissions for your Foundry resource. Since tokens may expire during long sessions, the system-assigned managed identity of the Foundry resource requires access to model deployments for the `byom-azure-openai-chat-completion` BYOM modes.

### Setup Commands

Run the following Azure CLI commands to configure the necessary permissions:

```bash
export subscription_id=<your-subscription-id>
export resource_group=<your-resource-group>
export foundry_resource=<your-foundry-resource>

# Enable system-assigned managed identity for the foundry resource
az cognitiveservices account identity assign --name ${foundry_resource} --resource-group ${resource_group} --subscription ${subscription_id}

# Get the system-assigned managed identity object ID
identity_principal_id=$(az cognitiveservices account show --name ${foundry_resource} --resource-group ${resource_group} --subscription ${subscription_id} --query "identity.principalId" -o tsv)

# Assign the Azure AI User role to the system identity of the foundry resource
az role assignment create --assignee-object-id ${identity_principal_id} --role "Azure AI User" --scope /subscriptions/${subscription_id}/resourceGroups/${resource_group}/providers/Microsoft.CognitiveServices/accounts/${foundry_resource}
```

For usage with Foundry resource overrides, this setup is mandatory. You need to run the setup commands for both the Voice Live Foundry resource and the model Foundry resource, as shown below:

```bash
export subscription_id_for_model=<your-subscription-id-for-model-resource>
export resource_group_for_model=<your-resource-group-for-model-resource>
export foundry_resource_for_model=<your-foundry-resource-for-model>    

export subscription_id_for_voice_live=<your-subscription-id-for-voice-live-resource>
export resource_group_for_voice_live=<your-resource-group-for-voice-live-resource>
export foundry_resource_for_voice_live=<your-foundry-resource-for-voice-live>

# Enable system-assigned managed identity for the voice live foundry resource
az cognitiveservices account identity assign --name ${foundry_resource_for_voice_live} --resource-group ${resource_group_for_voice_live} --subscription ${subscription_id_for_voice_live}

# Get the system-assigned managed identity object ID for voice live resource
identity_principal_id=$(az cognitiveservices account show --name ${foundry_resource_for_voice_live} --resource-group ${resource_group_for_voice_live} --subscription ${subscription_id_for_voice_live} --query "identity.principalId" -o tsv)    
# Assign the Azure AI User role to the system identity of the voice live foundry resource on the model foundry resource
az role assignment create --assignee-object-id ${identity_principal_id} --role "Azure AI User" --scope /subscriptions/${subscription_id_for_model}/resourceGroups/${resource_group_for_model}/providers/Microsoft.CognitiveServices/accounts/${foundry_resource_for_model}
```

## Step 4 — Use Voice Live SDK (VS Code)

### Prerequisites  
- VS Code (latest)  
- Python 3.9+  
- Azure CLI (`az login`)  

### Reference Documentation

- [Speech Service Quickstarts](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/quickstarts)  
- [BYOM Guide](https://learn.microsoft.com/en-us/azure/ai-services/speech-service/bring-own-model?tabs=python)_  
- [GitHub Sample]( https://github.com/hmntrathore/sampleVoiceLiveBYOM)



# Step 5 — BYOM API Call (REST Example)

Use your Voice Live BYOM endpoint:

```
wss://<your-foundry-resource>.cognitiveservices.azure.com/voice-live/realtime
   ?api-version=2025-10-01
   &profile=<your-byom-mode>
   &model=<your-model-deployment>
```

Where:

- **<your-byom-mode>** = byom-azure-openai-chat-completion OR byom-azure-openai-realtime  
- **<your-model-deployment>** = the Foundry model deployment name  




📁 [**Sample Code Repository:**](https://github.com/hmntrathore/sampleVoiceLiveBYOM)

# Benefits of Voice Live + BYOM

- Context-aware, domain-trained assistants  
- Predictable low-latency voice performance  
- Region-flexible architecture
- Fine‑tuned domain intelligence  
- Secure zero-key authentication  
- Dedicated PTUs for stable performance  

# Conclusion

Azure Voice Live API combined with BYOM offers enterprises:

- Greater accuracy  
- Full compliance  
- Secure zero-trust design  
- Flexible regional deployment  
- Dedicated performance via PTUs  

# References

- [Voice Live API Overview](https://learn.microsoft.com/azure/ai-services/voice-live/overview)
- [BYOM Guide](https://learn.microsoft.com/azure/ai-services/voice-live/byom)
- [Azure AI Foundry Docs](https://learn.microsoft.com/azure/ai-foundry)
- [Model Deployment Guide:](https://learn.microsoft.com/azure/ai-foundry/foundry-models/how-to/deploy-foundry-models)
- [Azure Speech Services](https://learn.microsoft.com/azure/ai-services/speech-service)
