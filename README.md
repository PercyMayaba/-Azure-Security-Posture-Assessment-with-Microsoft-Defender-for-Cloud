# Azure-Security Posture Assessment with Microsoft Defender for Cloud

This project focuses on assessing the security posture of an Azure subscription using Microsoft Defender for Cloud and Azure CLI. The objective was to identify security configuration gaps, review security recommendations, verify network security controls, and inspect security contact and notification settings.

This lab forms part of my Azure Cloud and AI Security Engineering portfolio, demonstrating practical experience with cloud security posture management, security assessments, identity and access governance, network security, and security monitoring configuration.

The exercise was performed within the `BonganiSecurity` resource group and its associated Azure environment. The lab prioritised read-only assessment and configuration verification to minimise unnecessary resource deployment and avoid enabling additional paid security services.

## Objectives

- Assess Azure subscription security recommendations.
- Interpret Healthy, Unhealthy, and NotApplicable assessment statuses.
- Review Microsoft Defender for Cloud recommendations.
- Verify network security group associations.
- Configure and verify security contact information.
- Review high-severity security alert notification settings.
- Identify security improvements while considering cost and licensing implications.

## Tools and Technologies

- Microsoft Azure Portal
- Azure Cloud Shell
- Azure CLI
- Microsoft Defender for Cloud
- Azure Security Assessments
- Azure Virtual Network
- Network Security Groups (NSGs)
- Azure subscription security contacts

## Environment

| Component | Configuration |
|---|---|
| Resource group | `BonganiSecurity` |
| Virtual network | `vnet-bongani-day2` |
| Subnet | `subnet-workload` |
| Network security group | `nsg-bongani-day2` |
| Security assessment records | 19 returned |
| Security contact | Configured and verified using Azure CLI |

## Implementation and Testing

### 1. Security Assessment Review

I used Azure CLI to retrieve the security assessment records available for my subscription.

```bash
az security assessment list \
  --query "length([])" \
  --output tsv
```

The command returned 19 assessment records.

I then queried the assessment names and statuses:

```bash
az security assessment list \
  --query "[].{Assessment:displayName, Status:status.code}" \
  --output table
```

The results included Healthy, Unhealthy, and NotApplicable statuses. These results provided an overview of the security recommendations that Azure identified for the subscription.

### 2. Assessment Findings

**Healthy findings**

The assessment output reported the following checks as Healthy:

- Maximum of three subscription owners.
- Network Watcher enabled.
- Subnets associated with a network security group.

The NSG finding was consistent with my earlier verification that `nsg-bongani-day2` was associated with `subnet-workload`.

**Unhealthy findings**

The assessment identified recommendations relating to:

- Having more than one owner assigned to the subscription.
- Enabling Microsoft Defender CSPM.
- Configuring email notifications for high-severity security alerts.
- Configuring a security contact email address.
- Enabling Microsoft Defender for Resource Manager.
- Enabling recommended Microsoft Defender for Storage protections.

These findings were treated as configuration recommendations requiring further review. An Unhealthy status does not, by itself, indicate that a security incident or compromise has occurred.

**NotApplicable findings**

Several other Defender recommendations were marked NotApplicable, including recommendations for APIs, SQL, App Service, Containers, Key Vault, and other services.

These results were documented as reported by Azure rather than interpreted as proof that the relevant protections were enabled.

### 3. Security Contact Verification

I reviewed the security contact and email notification settings in Microsoft Defender for Cloud and saved the notification configuration successfully.

I then used Azure CLI to verify the configured security contact:

```bash
az security contact list --output table
```

The command returned a contact named `default` with an email address configured.

This confirmed that a security contact record was available through the Azure security contact command. The email address and other personal contact details are intentionally omitted from this public repository.

The CLI displayed a preview notice, which indicates that the command is a preview feature. This notice did not prevent the command from returning the contact record.

### 4. Network Security Group Verification

As part of the assessment, I reviewed the network security recommendation concerning NSG associations.

The subnet-to-NSG association had previously been verified using:

```bash
az network vnet subnet show \
  --resource-group BonganiSecurity \
  --vnet-name vnet-bongani-day2 \
  --name subnet-workload \
  --query "{Subnet:name, NSG:networkSecurityGroup.id}" \
  --output json
```

The output confirmed that `subnet-workload` was associated with `nsg-bongani-day2`.

The security assessment subsequently reported the recommendation, “Subnets should be associated with a network security group,” as Healthy.

No virtual machine was deployed for packet-level testing in this lab.

## Cost and Security Considerations

I reviewed Defender for Cloud recommendations without automatically enabling additional protection plans.

Recommendations involving Defender CSPM, Defender for Resource Manager, and Defender for Storage require appropriate review of plan availability, pricing, coverage, and potential billing implications before activation.

The goal was to understand the subscription's security posture while keeping the lab cost-conscious.

An assessment recommendation is not sufficient evidence by itself to determine whether a feature incurs additional charges. Plan configuration and billing implications should be verified independently before making changes.

## Challenges and Observations

During assessment retrieval, Azure CLI repeatedly displayed the following warning:

`Discriminator source is absent or null, use base class ResourceDetails.`

Despite these warnings, the assessment command returned records and displayed the results table. The warnings were documented rather than treated as proof of a failed security assessment.

This exercise reinforced the importance of distinguishing CLI warnings from command failures and interpreting security recommendations in the context of verified resource configurations.

## Skills Demonstrated

- Azure security posture assessment.
- Azure CLI security resource queries.
- Interpretation of Azure security assessment statuses.
- Microsoft Defender for Cloud navigation and configuration review.
- Network security group association verification.
- Security contact configuration and verification.
- Security recommendation prioritisation.
- Cloud security cost awareness.
- Technical documentation and evidence-based reporting.

## Limitations

This was a subscription-level assessment and configuration exercise. It did not include penetration testing, a deployed workload, live attack simulation, or a full incident-response investigation.

The findings represent the assessment output observed during the lab. Security recommendations and resource states can change as the Azure environment changes.

## Next Steps

- Investigate remaining unhealthy recommendations.
- Review subscription ownership and least-privilege access requirements before making role changes.
- Confirm high-severity alert notification settings.
- Review Defender plan coverage and pricing before enabling additional services.
- Continue with Azure Activity Log investigation and security event analysis.
- Expand the portfolio with further Azure cloud and AI security engineering labs.


This lab provided hands-on experience with Azure security assessments, Defender for Cloud recommendations, network security verification, and security contact configuration.

It demonstrates an evidence-based approach to cloud security: assess the environment, verify configurations, document findings, and prioritise remediation while considering operational and financial risks.

**Portfolio:** BonganiSecurity Azure Cloud and AI Security Engineering Labs  
**Focus:** Cloud Security | Microsoft Azure | Security Posture Management | Network Security | Security Monitoring
