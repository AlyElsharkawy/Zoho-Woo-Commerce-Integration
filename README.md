# WooCommerce and Zoho CRM Integration Pipeline

An event-driven integration service built in Zoho Deluge to synchronize customer lifecycles and e-commerce orders between a WooCommerce storefront and Zoho CRM (v8 API) using custom webhooks and RESTful endpoints.

## Architecture and Refactoring Highlights

* **Refactored from Synchronous Polling to Asynchronous Webhook Pipelines:** Initially designed as a synchronous batch fetcher (`sync_woo_customers`), the pipeline was re-engineered to run asynchronously via HTTP webhooks triggered directly by WooCommerce events.
* **Benefits of Asynchronous Refactor:** 
  * Reduced latency and eliminated API rate-limiting bottlenecks on scheduled polling jobs.
  * Non-blocking execution ensuring real-time CRM state synchronization upon customer checkout or order update.
  * Improved error isolation and decoupled payload processing.

## Key Technical Features

* **Lead-to-Contact Auto-Conversion:** Automatically identifies returning customers via email lookup. Converts existing CRM Leads to full Contacts before creating and attaching new order deals.
* **Idempotent Record Updates:** Prevents duplicate record creation by searching for existing leads or contacts prior to executing creation or update operations.
* **Automated Deal Lifecycle Tracking:** Dynamically updates Deal stages in Zoho CRM (e.g., transition to "Closed Won" once the WooCommerce order status resolves to "completed").
* **Secure Data Mapping:** Integrates with Zoho OAuth2 connections and safely parses complex, nested JSON payloads (billing addresses, phone sanitization, line items).

## Tech Stack and Environment

* **Language:** Zoho Deluge
* **Target APIs:** Zoho CRM REST API (v8), WooCommerce REST API v3
* **Authentication:** OAuth2 / Named Connections (`woocommercecustom`)
* **Development and Testing:** Postman, WordPress / WooCommerce (Local Host / Linux Home Server)

## Repository Structure

* `Sync Woo Signup.dg`: Webhook listener processing user registrations and managing Lead records.
* `Get Woo Orders.dg`: Asynchronous order processor handling Lead-to-Contact conversion and Deal creation.
* `Update Woo Order.dg`: Listens for WooCommerce status changes and updates Deal stages in real time.
* `Old Sync Woo Customers.dg`: Legacy synchronous customer batch sync (retained for architectural comparison).
