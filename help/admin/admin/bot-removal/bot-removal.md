---
title: Bot removal in Adobe Analytics
seo-title: Bot removal in Adobe Analytics
description: 3 ways to remove bots in Adobe Analytics
seo-description: 3 ways to remove bots in Adobe Analytics
---

# Bot removal in Adobe Analytics

In Adobe Analytics, you have 3 main options for removing bot traffic from reporting:

1. The default bot filtering method in Adobe Analytics is to [create bot rules](/help/admin/admin/bot-removal/bot-rules.md) that are based on the IAB bot list. This list is updated monthly and compiles its list from many sources, including CDNs and major internet properties. It includes thousands of known bots including all of your favorites: Google, Bing, Mozilla, etc. This list covers the overwhelming majority of use cases and needs around bot filtration.

1. Use the [s.hitGovernor implementation plug-in](https://docs.adobe.com/content/help/en/analytics/implementation/javascript-implementation/plugins/hitgovernor.html), which removes visitors who behave like bots by sending dozens or hundreds of hits per minute.

1. In addition, since bots are advancing quickly, Adobe offers several other powerful features that, when combined properly and on a regular basis, can help drive the removal of these enemies of data quality. Those features are: Experience Cloud ID Service, Segmentation, Data Warehouse, Customer Attributes, and Virtual Report Suites. Here is an overview of how you can leverage these tools:

## Step 1: Pass your visitors’ Experience Cloud ID into a new declared ID

To start, you’ll want to create a new declared ID in the [Audiences core service](https://docs.adobe.com/content/help/en/core-services/interface/audiences/audience-library.html). You’ll need to pass your visitor’s Experience Cloud ID into this new declared ID, which can be done quickly and easily with [Adobe Experience Platform Launch](https://docs.adobe.com/content/help/en/launch/using/implement/solutions/idservice-save.html). Let's use the name “ECID” for the declared ID.

screenshot here

Here’s a screenshot of how this ID can be captured via Data Element. Be sure to populated your Adobe MCOrg ID into the Data Element correctly.

```return Visitor.getInstance("REPLACE_WITH_YOUR_ECORG_ID@AdobeOrg").getExperienceCloudVisitorID();```

Once this Data Element is set up, follow [these instructions](https://docs.adobe.com/content/help/en/launch/using/implement/solutions/idservice-save.html) to pass declared ID’s into the ECID Tool in Launch.

## Step 2: Use segmentation to identify bots

Now that you have your visitor’s ECID passed into a declared ID, it’s time to use segmentation in Analysis Workspace to identify visitors that are acting like bots. Bots are often defined by their behavior: single access visits, unusual user-agents, unknown device/browser information, no referrers, new visitors, unusual landing pages, etc. Use the powers of Workspace drilldowns and segmentation to identify the bots that have evaded IAB filtering and your report suite bot rules. For example, here’s a screenshot of a segment that you could use:

![](assets/bot-filter-seg1.png)

## Step 3: Export all the ECIDs from the segment via Data Warehouse

Now that you have identified the bots using segments, the next step is to leverage Data Warehouse to extract all the Experience Cloud IDs associated with this segment. This is how you should set up your [Data Warehouse](https://docs.adobe.com/content/help/en/analytics/export/data-warehouse/data-warehouse.html) report:

![](assets/bot-dwh-3.png)

Remember to use Experience Cloud Visitor ID as your dimension and apply the Bots segment.

## Step 4: Pass this list back to Adobe as a Customer Attribute

Once the Data Warehouse report arrives, you’ll have a list of ECIDs that need to be filtered from historical data. Copy and paste these ECIDs into a blank .CSV file with just two columns, ECID and Bot Flag:

![](assets/bot-csv-4.png)

Make sure the first column header matches the name you gave to the new declared ID above. Use this .CSV file as your Customer Attribute import file, then subscribe your report suite(s) to the Customer Attribute as described in this [blog post](https://theblog.adobe.com/link-digital-behavior-customers).

## Step 5: Create a segment that leverages the new Customer Attribute

Once your data set has been processed and integrated into Analysis Workspace, create one more segment that leverages your new “Bot Flag” customer attribute dimension:
