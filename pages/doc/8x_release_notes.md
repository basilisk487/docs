---
title: 8.x Release Notes
keywords:
tags: [release notes]
sidebar: doc_sidebar
permalink: 8x_release_notes.html
summary: Learn about new and updated features in Wavefront 8.x.
---
The Wavefront 8.x release provides a number of dashboard, documentation, and UI improvements:

- Updated Wavefront dashboards. The introductory and system dashboards have been updated and new demo dashboards have been added to your instance. For further info on these dashboards, see [Introductory Dashboards](dashboards_introductory) and [Monitoring the Health of a Wavefront Instance](wavefront_monitoring).
- All Wavefront dashboards now have tags in the **wavefront.*** hierarchy.  The dashboards have the following name and tagging conventions:
  - Intro: \<Name\> - wavefront.tutorial
  - Demo: \<Name\> - wavefront.tour
  - Wavefront Internal Metrics - wavefront.system

   {% include note.html content="All dashboards provided by Wavefront are now read-only. If you want to make changes to a Wavefront-provided dashboard you can clone it providing a new dashboard URL and then make changes to the clone. If  you already had these dashboards and you have edited them, we save a copy of your edited dashboards and add \"-clone\" to the URL and \"(Cloned)\" to the name." %}
- Various improvements to the "Populate your Data" flow
  - Simplified to 2 options (AWS integration and Wavefront proxy). See [AWS Metrics Integration](integrations_aws_metrics) and [Installing Wavefront Proxies](proxies_installing).
- Amazon Web Services improvements
  - **Dashboards** - After adding an AWS integration, newly updated AWS dashboards—Overall, Billing, EBS Storage, Instance Storage, and Network—are added to your instance. These dashboards provide insight into AWS usage, enabling you to control your costs and maximize utilization of AWS services.
  - **Metrics** - The [aggregations](integrations_aws_metrics#aws-aggregate-metrics) average, minimum, maximum, samplecount, and sum are now available for all metrics and are enabled by default. The aws.instance.price metric now has a point tag, operatingSystem, to indicate whether the AWS instance is running Windows or Linux.
- A new Help panel at the right side bar on every page streamlines the on-boarding process for new Wavefront users.
- The dashboard documentation now covers how to clone and deploy dashboards. 
- In the UI, the term "Proxy" is now used instead of "Agent"


{% include links.html %}