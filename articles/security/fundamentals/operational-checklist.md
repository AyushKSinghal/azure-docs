---

title: Azure operational security checklist| Microsoft Docs
description: Review this checklist to help your enterprise think through Azure operational security considerations.
services: security
documentationcenter: na
author: unifycloud
manager: barbkess
editor: tomsh

ms.assetid:
ms.service: information-protection
ms.subservice: aiplabels
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: tomsh

---

# Azure operational security checklist
Deploying an application on Azure  is fast, easy, and cost-effective. Before deploying cloud application in production useful to have a checklist to assist in evaluating your application against a list of essential and recommended operational security actions for you to consider.

## Introduction

Azure provides a suite of infrastructure services that you can use to deploy your applications. Azure Operational Security refers to the services, controls, and features available to users for protecting their data, applications, and other assets in Microsoft Azure.

-	To get the maximum benefit out of the cloud platform, we recommend that you leverage Azure services and follow the checklist.
-	Organizations that invest time and resources assessing the operational readiness of their applications before launch have a much higher rate of satisfaction than those who don’t. When performing this work, checklists can be an invaluable mechanism to ensure that applications are evaluated consistently and holistically.
-	The level of operational assessment varies depending on the organization’s cloud maturity level and the application’s development phase, availability needs, and data sensitivity requirements.

## Checklist

This checklist is intended to help enterprises think through various operational security considerations as they deploy sophisticated enterprise applications on Azure. It can also be used to help you build a secure cloud migration and operation strategy for your organization.

|Checklist Category| Description|
| ------------ | -------- |
| [<br>Security Roles & Access Controls](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Use [Azure role-based access control (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) to provide user-specific that used to assign permissions to users, groups, and applications at a certain scope.</li></ul> |
| [<br>Data Collection & Storage](../../storage/blobs/security-recommendations.md)|<ul><li>Use Management Plane Security to secure your Storage Account using [Azure role-based access control (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md).</li><li>Data Plane Security to Securing Access to your Data using [Shared Access Signatures (SAS)](../../storage/common/storage-sas-overview.md) and Stored Access Policies.</li><li>Use Transport-Level Encryption – Using HTTPS and the encryption used by [SMB (Server message block protocols) 3.0](/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview) for [Azure File Shares](../../storage/files/storage-dotnet-how-to-use-files.md).</li><li>Use [Client-side encryption](../../storage/common/storage-client-side-encryption.md) to secure data that you send to storage accounts when you require sole control of encryption keys. </li><li>Use [Storage Service Encryption (SSE)](../../storage/common/storage-service-encryption.md)  to automatically encrypt data in Azure Storage, and [Azure Disk Encryption for Linux VMs](../../virtual-machines/linux/disk-encryption-overview.md) and [Azure Disk Encryption for Windows VMs](../../virtual-machines/linux/disk-encryption-overview.md) to encrypt virtual machine disk files for the OS and data disks.</li><li>Use Azure [Storage Analytics](/rest/api/storageservices/storage-analytics) to monitor authorization type; like with Blob Storage, you can see if users have used a Shared Access Signature or the storage account keys.</li><li>Use [Cross-Origin Resource Sharing (CORS)](/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services) to access storage resources from different domains.</li></ul> |
|[<br>Security Policies & Recommendations](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Use [Microsoft Defender for Cloud](../../security-center/security-center-services.md#supported-endpoint-protection-solutions-) to deploy endpoint solutions.</li><li>Add a [web application firewall (WAF)](../../web-application-firewall/ag/ag-overview.md) to secure web applications.</li><li>	Use a [firewall](../../sentinel/connect-data-sources.md) from a Microsoft partner to increase your security protections. </li><li>Apply security contact details for your Azure subscription; this the [Microsoft Security Response Center (MSRC)](https://technet.microsoft.com/security/dn528958.aspx) contacts you if it discovers that your customer data has been accessed by an unlawful or unauthorized party.</li></ul> |
| [<br>Identity & Access Management](identity-management-best-practices.md)|<ul><li>[Synchronize your on-premises directory with your cloud directory using Azure AD](../../active-directory/hybrid/whatis-hybrid-identity.md).</li><li>Use [single sign-on](https://azure.microsoft.com/resources/videos/overview-of-single-sign-on/) to enable users to access their SaaS applications based on their organizational account in Azure AD.</li><li>Use the [Password Reset Registration Activity](../../active-directory/authentication/howto-sspr-reporting.md) report to monitor the users that are registering.</li><li>Enable [multi-factor authentication (MFA)](../../active-directory/authentication/concept-mfa-howitworks.md) for users.</li><li>Developers to use secure identity capabilities for apps like [Microsoft Security Development Lifecycle (SDL)](https://www.microsoft.com/download/details.aspx?id=12379).</li><li>Actively monitor for suspicious activities by using Azure AD Premium anomaly reports and [Azure AD identity protection capability](../../active-directory/identity-protection/overview-identity-protection.md).</li></ul> |
|[<br>Ongoing Security Monitoring](../../security-center/security-center-planning-and-operations-guide.md)|<ul><li>Use Malware Assessment Solution [Azure Monitor logs](../../azure-monitor/logs/log-query-overview.md) to report on the status of antimalware protection in your infrastructure.</li><li>Use [Update assessment](../../automation/update-management/overview.md) to determine the overall exposure to potential security problems, and whether or how critical these updates are for your environment.</li><li>The [Identity and Access](../../security-center/security-center-remediate-recommendations.md) provide you an overview of user </li><ul><li>user identity state,</li><li>number of failed attempts to sign in,</li><li>	the user’s account that were used during those attempts, accounts that were locked out</li> <li>accounts with changed or reset password </li><li>Currently number of accounts that are logged in.</li></ul></ul> |
| [<br>Microsoft Defender for Cloud detection capabilities](../../security-center/security-center-alerts-overview.md#detect-threats)|<ul><li>Use [detection capabilities](../../security-center/security-center-alerts-overview.md#detect-threats), to identify active threats targeting your Microsoft Azure resources.</li><li>Use [integrated threat intelligence](/archive/blogs/azuresecurity/get-threat-intelligence-reports-with-azure-security-center) that looks for known bad actors by leveraging global threat intelligence from Microsoft products and services, the [Microsoft Digital Crimes Unit (DCU)](https://www.microsoft.com/trustcenter/security/cybercrime), the [Microsoft Security Response Center (MSRC)](https://www.microsoft.com/msrc?rtc=1), and external feeds.</li><li>Use [Behavioral analytics](https://blogs.technet.microsoft.com/enterprisemobility/2016/06/30/ata-behavior-analysis-monitoring/) that applies known patterns to discover malicious behavior. </li><li>Use [Anomaly detection](/azure/machine-learning/studio-module-reference/anomaly-detection) that uses statistical profiling to build a historical baseline.</li></ul> |
| [<br>Developer Operations (DevOps)](/azure/architecture/checklist/dev-ops)|<ul><li>[Infrastructure as Code (IaC)](../../azure-resource-manager/templates/syntax.md) is a practice, which enables the automation and validation of creation and teardown of networks and virtual machines to help with delivering secure, stable application hosting platforms.</li><li>[Continuous Integration and Deployment](/visualstudio/containers/overview#continuous-delivery-and-continuous-integration-cicd) drive the ongoing merging and testing of code, which leads to finding defects early. </li><li>[Release Management](/azure/devops/pipelines/overview?viewFallbackFrom=azure-devops) Manage automated deployments through each stage of your pipeline.</li><li>[App Performance Monitoring](../../azure-monitor/app/asp-net.md) of running applications including production environments for application health and customer usage help organizations form a hypothesis and quickly validate or disprove strategies.</li><li>Using [Load Testing & Auto-Scale](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing) we can find performance problems in our app to improve deployment quality and to make sure our app is always up or available to cater to the business needs.</li></ul> |


## Conclusion
Many organizations have successfully deployed and operated their cloud applications on Azure. The checklists provided highlight several checklists that are essential and help you to increase the likelihood of successful deployments and frustration-free operations. We highly recommend these operational and strategic considerations for your existing and new application deployments on Azure.

## Next steps
To learn more about Security, see the following articles:

- [Design and operational security](https://www.microsoft.com/trustcenter/security/designopsecurity).
- [Microsoft Defender for Cloud planning and operations](../../security-center/security-center-planning-and-operations-guide.md).
