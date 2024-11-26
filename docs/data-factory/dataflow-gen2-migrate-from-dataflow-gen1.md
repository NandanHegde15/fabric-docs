---
title: "Migrate from Dataflow Gen1 to Dataflow Gen2"
description: "Guidance to help you migrate your Dataflow Gen1 to Dataflow Gen2 in Data Factory for Microsoft Fabric."
author: itsnotaboutthecell
ms.author: alpowers
ms.reviewer: dougklopfenstein, mllopis
ms.topic: conceptual
ms.date: 11/26/2024
ms.custom: fabric-cat, intro-migration
---

# Migrate from Dataflow Gen1 to Dataflow Gen2

This article targets Power BI dataflow creators. It provides them with guidance and rationale to help migrate their dataflows to Dataflow Gen2 in [Data Factory](data-factory-overview.md) for Microsoft Fabric.

> [!NOTE]
> Dataflow Gen2 is a new generation of dataflows that delivers new features and improved experiences. Gen2 dataflows reside alongside Power BI dataflows, which are now known as _Dataflow Gen1_.
>
> To understand the differences between Dataflow Gen1 and Dataflow Gen2, see [Getting from Dataflow Generation 1 to Dataflow Generation 2](dataflows-gen2-overview.md).

## Background

Microsoft Fabric has evolved into an integrated platform for both self-service and IT-managed enterprise data. With exponential growth in data volumes and complexity, Fabric customers demand that their enterprise solutions scale, are secure, easy to manage, and accessible to all users across the largest of organizations.

In recent years, Microsoft has taken great strides to deliver scalable cloud capabilities to [Fabric capacity](../enterprise/licenses.md#capacity). To that end, Data Factory in Fabric instantly empowers a large ecosystem of data integration developers and data integration solutions that have been built up over decades. It leverages the full set of features and capabilities that go far beyond comparable functionality available in previous generations.

Naturally, customers are now asking whether there's an opportunity to consolidate their data integration solutions by hosting them within Fabric. They often ask questions like:

- Does all the dataflow functionality we depend on work in Dataflow Gen2?
- What capabilities are available only in Dataflow Gen2?
- How do we migrate existing dataflows to Dataflow Gen2?
- What's Microsoft's roadmap for enterprise data ingestion?

Answers to many of these questions are described in this article.

> [!NOTE]
> The decision to migrate to Fabric capacity depends on the requirements of each customer. Customers should carefully evaluate the benefits in order to make an informed decision. We expect to see organic migration to Dataflow Gen2 over time, and our intention is that it happens on terms that the customer is comfortable with.
>
> To be clear, currently there aren't any plans to deprecate Power BI dataflows or Power Platform dataflows. However, there is a priority to focus investment on Dataflow Gen2 for enterprise data ingestion, and so the value provided by Fabric capacity will increase over time. Customers that choose Fabric capacity can expect to benefit from alignment with the [Microsoft Fabric product roadmap](/fabric/release-plan/data-factory).

### Convergence of self-service and enterprise data integration

The consolidation of items in Fabric simplifies discovery, collaboration, and management by co-locating resources. It allows central IT teams to more easily adopt and integrate popular self-service items. At the same time, it allows operationalizing mission-critical data movement and transformation services aligned with corporate standards, including data lineage and monitoring.

To support the collaborative and scalable needs of creators, Dataflow Gen2 in Fabric introduces [fast copy](dataflows-gen2-fast-copy.md), which enables efficient ingestion of large data volumes by using Fabric's backend infrastructure to [store and process](data-in-staging-artifacts.md) intermediate data during transformation. It can handle terabytes of data seamlessly. Dataflow creators can specify [data destinations](dataflow-gen2-data-destinations-and-managed-settings.md) for their transformed data, such as a Fabric lakehouse, warehouse, eventhouse, or Azure SQL Database, facilitating better data management and accessibility. And what's more, the recent integration of generative AI through [Copilot](../get-started/copilot-fabric-data-factory.md) enhances the data preparation experience by providing intelligent code generation and automating repetitive tasks, providing an easier and faster path to create complex solutions.

By utilizing a common platform, the workflow is streamlined, which results in enhanced collaboration between the business and IT. Organizations are therefore empowered to scale their data solutions to enterprise levels, ensuring high performance, flexibility, and efficiency in managing vast volumes of data.

### Fabric capacity

Thanks to its distributed architecture, [Fabric capacity](../enterprise/licenses.md#capacity) is less sensitive to overall load, temporal spikes, and high concurrency. By consolidating capacities to larger Fabric capacity SKUs, customers can achieve increased performance and throughput.

## Feature comparison

The following table presents features supported in Power BI dataflow and/or Fabric Dataflow Gen2.

| Feature | Power BI Dataflow Gen1 | Fabric Dataflow Gen2 |
|:-|:-:|:-:|
| **Connectivity** |||
| Support for all [Power Query data sources](/power-query/connectors/) | Yes | Yes |
| Connect to, and load data from, dataflows in Power BI Desktop, Excel, or Power Apps | Yes | Yes |
| **Scalability** |||
| [Fast copy](dataflows-gen2-fast-copy.md), which supports large-scale data ingestion, utilizing the data pipeline [Copy activity](copy-data-activity.md) within dataflows | No | Yes |
| [Scheduled refresh](dataflow-gen2-refresh.md), which keeps data current | Yes | Yes |
| [Incremental refresh](dataflow-gen2-incremental-refresh.md), which uses policies to automate incremental data load and can help deliver near real-time reporting | Yes | Yes |
| [Data pipeline orchestration](dataflow-activity.md), which allows you to add a [Dataflow activity](dataflow-activity.md) to a data pipeline and create orchestrated conditional events | No | Yes |
| **Artificial intelligence** |||
| [Copilot for Data Factory](../get-started/copilot-fabric-data-factory.md), which provides intelligent code generation to transform data with ease, and generates code explanations to help better understand complex tasks | No | Yes |
| [Cognitive Services](/power-bi/transform-model/dataflows/dataflows-machine-learning-integration), which use artificial intelligence (AI) to apply different algorithms from Azure Cognitive Services to enrich self-service data preparation | Yes | No <sup>1</sup> |
| [Automated machine learning (AutoML)](/power-bi/transform-model/dataflows/dataflows-machine-learning-integration), which enables business analysts to train, validate, and invoke machine learning (ML) models directly in Fabric | Deprecated <sup>2</sup> ||
| [Azure Machine Learning](/power-bi/transform-model/dataflows/dataflows-machine-learning-integration) integration, which exposes custom models as dynamic Power Query functions that users can invoke in the Power Query Editor | Yes | No <sup>1</sup> |
| **Content management** |||
| [Data lineage view](../governance/lineage.md), which help users understand and assess dataflow item dependencies | Yes | Yes |
| [Deployment pipelines](../cicd/deployment-pipelines/get-started-with-deployment-pipelines.md), which manage the lifecycle of Fabric content | Yes | Yes |
| **Platform scalability and resiliency** |||
| [Premium capacity](../enterprise/licenses.md) architecture, which supports increased scale and performance | Yes | Yes |
| [Multi-Geo](../admin/service-admin-premium-multi-geo.md) support, which helps multinational customers address regional, industry-specific, or organizational data residency requirements | Yes <sup>3</sup> | Yes |
| **Security** |||
| [Virtual network (VNet) data gateway](/data-integration/vnet/overview) connectivity, which allows Fabric to work seamlessly in an organization's virtual network | No | Yes |
| [On-premises data gateway](/data-integration/gateway/service-gateway-onprem) connectivity, which allows for secure access of data between an organization's on-premises data sources and Fabric | Yes | Yes |
| Azure [service tags](../security/security-service-tags.md) support, which is a defined group of IP addresses that's automatically managed to minimize the complexity of updates or changes to network security rules | Yes | Yes |
| **Governance** |||
| Content [endorsement](../governance/endorsement-overview.md), to promote or certify valuable, high-quality Fabric items | Yes | Yes |
| [Microsoft Purview integration](../governance/microsoft-purview-fabric.md), which helps customers manage and govern Fabric items | Yes | Yes |
| Microsoft Information Protection (MIP) [sensitivity labels](../get-started/apply-sensitivity-labels.md) and integration with [Microsoft Defender for Cloud Apps](../governance/service-security-using-defender-for-cloud-apps-controls.md) for data loss prevention (DLP) | Yes | Yes |
| **Monitoring and diagnostic logging** |||
| Enhanced [refresh history](dataflows-gen2-monitor.md), which allows you to evaluate in detail what happened during the refresh of your dataflow | No | Yes |
| [Monitoring hub](../admin/monitoring-hub.md), which provides monitoring capabilities for Fabric items | No | Yes |
| [Microsoft Fabric Capacity Metrics app](../enterprise/metrics-app.md), which provides monitoring capabilities for Fabric capacity | Yes | Yes |
| [Audit log](../admin/track-user-activities.md), which tracks user activities across Fabric and Microsoft 365 | Yes | Yes |

<sup>1</sup> To learn how to create custom functions that call Azure AI API endpoints, see [Tutorial: Extract key phrases from text stored in Power BI](/azure/ai-services/language-service/key-phrase-extraction/tutorials/integrate-power-bi).

<sup>2</sup> Automated Machine Learning (AutoML) has been deprecated. For more information, see [this official announcement](https://powerbi.microsoft.com/blog/deprecation-of-automl-in-power-bi-using-dataflows-v1/).

<sup>3</sup> To configure Power BI dataflow storage to use Azure Data Lake Storage (ADLS) Gen2, see [this article](/power-bi/transform-model/dataflows/dataflows-azure-data-lake-storage-integration).

## Considerations

There are other considerations to factor into your planning before migrating to Dataflow Gen2.

### Licensing

You require a Pro or Premium Per User (PPU) license to publish or manage Power BI dataflows (Dataflow Gen1). In contrast, you only require a Microsoft Fabric (Free) license to author a Dataflow Gen2 in a Premium capacity workspace.

### Migration

[Power Query templates](/power-query/power-query-template) simplify the process of transferring a project between different Power Query integrations. They help streamline what could otherwise be a complex and time-consuming task. Templates encapsulate the entire Power Query project, including scripts and metadata, into a single, portable file.

Power Query templates have been designed to be compatible with various integrations, such as Power BI dataflows and Fabric Dataflow Gen2, ensuring a smooth transition between these services.

### Roadmap

The [Microsoft Fabric release plan](https://aka.ms/fabricreleaseplan) announces the latest updates and timelines as features are prepared for future release, including what's new and planned for [Data Factory in Microsoft Fabric](/fabric/release-plan/data-factory).

## Related content

For more information about this article, check out the following resources:

- [What is Data Factory in Microsoft Fabric?](data-factory-overview.md)
- [Getting from Dataflow Generation 1 to Dataflow Generation 2](dataflows-gen2-overview.md)
- Questions? [Try asking the Fabric community](https://community.fabric.microsoft.com/)
- Suggestions? [Contribute ideas to improve Fabric](https://ideas.fabric.microsoft.com)