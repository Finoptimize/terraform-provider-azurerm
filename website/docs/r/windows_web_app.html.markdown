---
subcategory: "App Service (Web Apps)"
layout: "azurerm"
page_title: "Azure Resource Manager: azurerm_windows_web_app"
description: |-
  Manages a Windows Web App.
---

# azurerm_windows_web_app

Manages a Windows Web App.

!> **NOTE:** This is a 3.0 beta resource intended to provide an improved experience for Web Apps by splitting the older `azurerm_app_service` into O/S Specific variants, allowing better property validation and simpler configuration. Please see the [3.0 Beta guide](https://github.com/hashicorp/terraform-provider-azurerm/blob/f/main/website/docs/guides/3.0-beta.html.markdown) for more information on enabling and using this resource.

## Example Usage

```hcl
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "West Europe"
}

resource "azurerm_service_plan" "example" {
  name                = "example"
  resource_group_name = azurerm_resource_group.example.name
  location            = "West Europe"
  sku_name            = "P1V2"
}

resource "azurerm_windows_web_app" "example" {
  name                = "example"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_service_plan.example.location
  service_plan_id     = azurerm_service_plan.example.id

  site_config {}
}
```

## Arguments Reference

The following arguments are supported:

* `location` - (Required) The Azure Region where the Windows Web App should exist. Changing this forces a new Windows Web App to be created.

* `name` - (Required) The name which should be used for this Windows Web App. Changing this forces a new Windows Web App to be created.

* `resource_group_name` - (Required) The name of the Resource Group where the Windows Web App should exist. Changing this forces a new Windows Web App to be created.

* `service_plan_id` - (Required) The ID of the Service Plan that this Windows App Service will be created in.

* `site_config` - (Required) A `site_config` block as defined below.

---

* `app_settings` - (Optional) A map of key-value pairs of App Settings.

* `auth_settings` - (Optional) A `auth_settings` block as defined below.

* `backup` - (Optional) A `backup` block as defined below.

* `client_affinity_enabled` - (Optional) Should Client Affinity be enabled?

* `client_cert_enabled` - (Optional) Should Client Certificates be enabled?

* `client_cert_mode` - (Optional) The Client Certificate mode. Possible values include `Optional` and `Required`. This property has no effect when `client_cert_enabled` is `false`

* `connection_string` - (Optional) One or more `connection_string` blocks as defined below.

* `enabled` - (Optional) Should the Windows Web App be enabled? Defaults to `true`.

* `https_only` - (Optional) Should the Windows Web App require HTTPS connections.

* `identity` - (Optional) A `identity` block as defined below.

* `logs` - (Optional) A `logs` block as defined below.

* `storage_account` - (Optional) One or more `storage_account` blocks as defined below.

* `tags` - (Optional) A mapping of tags which should be assigned to the Windows Web App.

---

A `action` block supports the following:

* `action_type` - (Required) Predefined action to be taken to an Auto Heal trigger. Possible values include: `Recycle`, `LogEvent`, and `CustomAction`.

* `custom_action` - (Optional) A `custom_action` block as defined below.

* `minimum_process_execution_time` - (Optional) The minimum amount of time in `hh:mm:ss` the Windows Web App must have been running before the defined action will be run in the event of a trigger.

---

A `active_directory` block supports the following:

* `client_id` - (Required) The ID of the Client to use to authenticate with Azure Active Directory.

* `allowed_audiences` - (Optional) Specifies a list of Allowed audience values to consider when validating JWTs issued by Azure Active Directory. 

~> **Note:** The `client_id` value is always considered an allowed audience.

* `client_secret` - (Optional) The Client Secret for the Client ID. Cannot be used with `client_secret_setting_name`. 

* `client_secret_setting_name` - (Optional) The App Setting name that contains the client secret of the Client. Cannot be used with `client_secret`.

---

A `application_logs` block supports the following:

* `azure_blob_storage` - (Optional) A `azure_blob_storage` block as defined below.

* `file_system_level` - (Optional) Log level. Possible values include: `Verbose`, `Information`, `Warning`, and `Error`.

---

A `application_stack` block supports the following:

* `current_stack` - (Optional) The Application Stack for the Windows Web App. Possible values include `dotnet`, `node`, `python`, `php`, and `java`.

~> **NOTE:** Whilst this property is Optional omitting it can cause unexpected behaviour, in particular for display of settings in the Azure Portal.

* `docker_container_name` - (Optional) The name of the Docker Container. For example `azure-app-service/samples/aspnethelloworld`

* `docker_container_registry` - (Optional) The registry Host on which the specified Docker Container can be located. For example `mcr.microsoft.com`

* `docker_container_tag` - (Optional) The Image Tag of the specified Docker Container to use. For example `latest`

* `dotnet_framework_version` - (Optional) The version of .Net to use when `current_stack` is set to `dotnet`. Possible values include `v2.0`, `v3.0`, `v4.0`, and `v5.0`.

* `java_container` - (Optional) The Java container type to use when `current_stack` is set to `java`. Possible values include `JAVA`, `JETTY`, and `TOMCAT`. Required with `java_version` and `java_container_version`.

* `java_container_version` - (Optional) The Version of the `java_container` to use. Required with `java_version` and `java_container`.

* `java_version` - (Optional) The version of Java to use when `current_stack` is set to `java`. Possible values include `1.7`, `1.8` and `11`. Required with `java_container` and `java_container_version`.

~> **NOTE:** For compatible combinations of `java_version`, `java_container` and `java_container_version` users can use `az webapp list-runtimes` from command line.

* `node_version` - (Optional) The version of node to use when `current_stack` is set to `node`.

~> **NOTE:** This property conflicts with `java_version`.

* `php_version` - (Optional) The version of PHP to use when `current_stack` is set to `php`. Possible values include `v5.6`, `v7.3`, and `v7.4`.

* `python_version` - (Optional) The version of Python to use when `current_stack` is set to `python`. Possible values include `2.7` and `3.4.0`.

---

A `auth_settings` block supports the following:

* `enabled` - (Required) Should the Authentication / Authorization feature is enabled for the Windows Web App be enabled?

* `active_directory` - (Optional) An `active_directory` block as defined above.

* `additional_login_params` - (Optional) Specifies a map of Login Parameters to send to the OpenID Connect authorization endpoint when a user logs in. 

* `allowed_external_redirect_urls` - (Optional) Specifies a list of External URLs that can be redirected to as part of logging in or logging out of the Windows Web App.

* `default_provider` - (Optional) The default authentication provider to use when multiple providers are configured. Possible values include: `BuiltInAuthenticationProviderAzureActiveDirectory`, `BuiltInAuthenticationProviderFacebook`, `BuiltInAuthenticationProviderGoogle`, `BuiltInAuthenticationProviderMicrosoftAccount`, `BuiltInAuthenticationProviderTwitter`, `BuiltInAuthenticationProviderGithub`

~> **NOTE:** This setting is only needed if multiple providers are configured, and the `unauthenticated_client_action` is set to "RedirectToLoginPage".

* `facebook` - (Optional) A `facebook` block as defined below.

* `github` - (Optional) A `github` block as defined below.

* `google` - (Optional) A `google` block as defined below.

* `issuer` - (Optional) The OpenID Connect Issuer URI that represents the entity which issues access tokens for this Windows Web App.

~> **NOTE:** When using Azure Active Directory, this value is the URI of the directory tenant, e.g. https://sts.windows.net/{tenant-guid}/.

* `microsoft` - (Optional) A `microsoft` block as defined below.

* `runtime_version` - (Optional) The RuntimeVersion of the Authentication / Authorization feature in use for the Windows Web App.

* `token_refresh_extension_hours` - (Optional) The number of hours after session token expiration that a session token can be used to call the token refresh API. Defaults to `72` hours. 

* `token_store_enabled` - (Optional) Should the Windows Web App durably store platform-specific security tokens that are obtained during login flows? Defaults to `false`.

* `twitter` - (Optional) A `twitter` block as defined below.

* `unauthenticated_client_action` - (Optional) The action to take when an unauthenticated client attempts to access the app. Possible values include: `RedirectToLoginPage`, `AllowAnonymous`.

---

A `auto_heal_setting` block supports the following:

* `action` - (Required) A `action` block as defined above.

* `trigger` - (Required) A `trigger` block as defined below.

---

A `azure_blob_storage` block supports the following:

* `retention_in_days` - (Required) The time in days after which to remove blobs. A value of `0` means no retention.

* `sas_url` - (Required) SAS url to an Azure blob container with read/write/list/delete permissions.

---

A `backup` block supports the following:

* `name` - (Required) The name which should be used for this Backup.

* `schedule` - (Required) A `schedule` block as defined below.

* `storage_account_url` - (Required) The SAS URL to the container.

* `enabled` - (Optional) Should this backup job be enabled?

---

A `connection_string` block supports the following:

* `type` - (Required) Type of database. Possible values include: `MySQL`, `SQLServer`, `SQLAzure`, `Custom`, `NotificationHub`, `ServiceBus`, `EventHub`, `APIHub`, `DocDb`, `RedisCache`, and `PostgreSQL`.

* `value` - (Required) The connection string value.

---

A `cors` block supports the following:

* `allowed_origins` - (Required) Specifies a list of origins that should be allowed to make cross-origin calls.

* `support_credentials` - (Optional) Whether CORS requests with credentials are allowed. Defaults to `false`

---

A `custom_action` block supports the following:

* `executable` - (Required) The executable to run for the `custom_action`.

* `parameters` - (Optional) The parameters to pass to the specified `executable`.

---

A `facebook` block supports the following:

* `app_id` - (Required) The App ID of the Facebook app used for login.

* `app_secret` - (Optional) The App Secret of the Facebook app used for Facebook Login. Cannot be specified with `app_secret_setting_name`.

* `app_secret_setting_name` - (Optional) The app setting name that contains the `app_secret` value used for Facebook Login. Cannot be specified with `app_secret`. 

* `oauth_scopes` - (Optional) Specifies a list of OAuth 2.0 scopes to be requested as part of Facebook Login authentication.

---

A `file_system` block supports the following:

* `retention_in_days` - (Required) The retention period in days. A values of `0` means no retention.

* `retention_in_mb` - (Required) The maximum size in megabytes that log files can use.

---

A `github` block supports the following:

* `client_id` - (Required) The ID of the GitHub app used for login.

* `client_secret` - (Optional) The Client Secret of the GitHub app used for GitHub Login. Cannot be specified with `client_secret_setting_name`.

* `client_secret_setting_name` - (Optional) The app setting name that contains the `client_secret` value used for GitHub Login. Cannot be specified with `client_secret`.

* `oauth_scopes` - (Optional) Specifies a list of OAuth 2.0 scopes that will be requested as part of GitHub Login authentication.

---

A `google` block supports the following:

* `client_id` - (Required) The OpenID Connect Client ID for the Google web application.

* `client_secret` - (Optional) The client secret associated with the Google web application.  Cannot be specified with `client_secret_setting_name`.

* `client_secret_setting_name` - (Optional) The app setting name that contains the `client_secret` value used for Google Login. Cannot be specified with `client_secret`.

* `oauth_scopes` - (Optional) Specifies a list of OAuth 2.0 scopes that will be requested as part of Google Sign-In authentication. If not specified, "openid", "profile", and "email" are used as default scopes. 

---

A `headers` block supports the following:

~> **NOTE:** Please see the [official Azure Documentation](https://docs.microsoft.com/en-us/azure/app-service/app-service-ip-restrictions#filter-by-http-header) for details on using header filtering.

* `x_azure_fdid` - (Optional) Specifies a list of Azure Front Door IDs.

* `x_fd_health_probe` - (Optional) Specifies if a Front Door Health Probe should be expected.

* `x_forwarded_for` - (Optional) Specifies a list of addresses for which matching should be applied. Omitting this value means allow any.

* `x_forwarded_host` - (Optional) Specifies a list of Hosts for which matching should be applied. 

---

A `http_logs` block supports the following:

* `azure_blob_storage` - (Optional) A `azure_blob_storage` block as defined above.

* `file_system` - (Optional) A `file_system` block as defined above.

---

A `identity` block supports the following:

* `type` - (Required) The type of managed service identity. Possible values include: `ManagedServiceIdentityTypeSystemAssigned`, `ManagedServiceIdentityTypeUserAssigned`, and `ManagedServiceIdentityTypeSystemAssignedUserAssigned`.

* `identity_ids` - (Optional) Specifies a list of Identity IDs.

---

A `ip_restriction` block supports the following:

* `action` - (Optional) The action to take. Possible values are `Allow` or `Deny`.

* `headers` - (Optional) A `headers` block as defined above.

* `ip_address` - (Optional) The CIDR notation of the IP or IP Range to match. For example: `10.0.0.0/24` or `192.168.10.1/32`

* `name` - (Optional) The name which should be used for this `ip_restriction`.

* `priority` - (Optional) The priority value of this `ip_restriction`. 

* `service_tag` - (Optional) The Service Tag used for this IP Restriction.

* `virtual_network_subnet_id` - (Optional) The Virtual Network Subnet ID used for this IP Restriction.

~> **NOTE:** One and only one of `ip_address`, `service_tag` or `virtual_network_subnet_id` must be specified.

---

A `logs` block supports the following:

* `application_logs` - (Optional) A `application_logs` block as defined above.

* `detailed_error_messages` - (Optional) Should detailed error messages be enabled.

* `failed_request_tracing` - (Optional) Should tracing be enabled for failed requests.

* `http_logs` - (Optional) A `http_logs` block as defined above.

---

A `microsoft` block supports the following:

* `client_id` - (Required) The OAuth 2.0 client ID that was created for the app used for authentication.

* `client_secret` - (Optional) The OAuth 2.0 client secret that was created for the app used for authentication. Cannot be specified with `client_secret_setting_name`.
  
* `client_secret_setting_name` - (Optional) The app setting name containing the OAuth 2.0 client secret that was created for the app used for authentication. Cannot be specified with `client_secret`.

* `oauth_scopes` - (Optional) Specifies a list of OAuth 2.0 scopes that will be requested as part of Microsoft Account authentication. If not specified, "wl.basic" is used as the default scope.

---

A `requests` block supports the following:

* `count` - (Required) The number of requests in the specified `interval` to trigger this rule.

* `interval` - (Required) The interval in `hh:mm:ss`. 

---

A `schedule` block supports the following:

* `frequency_interval` - (Required) How often the backup should be executed (e.g. for weekly backup, this should be set to `7` and `frequency_unit` should be set to `Day`).

~> **NOTE:** Not all intervals are supported on all Windows Web App SKU's. Please refer to the official documentation for appropriate values.

* `frequency_unit` - (Required) The unit of time for how often the backup should take place. Possible values include: `Day`, `Hour`

* `keep_at_least_one_backup` - (Optional) Should the service keep at least one backup, regardless of age of backup. Defaults to `false`.

* `retention_period_days` - (Optional) After how many days backups should be deleted.

* `start_time` - (Optional) When the schedule should start working in RFC-3339 format.

---

A `scm_ip_restriction` block supports the following:

* `action` - (Optional) The action to take. Possible values are `Allow` or `Deny`.

* `headers` - (Optional) A `headers` block as defined above.

* `ip_address` - (Optional) The CIDR notation of the IP or IP Range to match. For example: `10.0.0.0/24` or `192.168.10.1/32`

* `name` - (Optional) The name which should be used for this `ip_restriction`.

* `priority` - (Optional) The priority value of this `ip_restriction`.

* `service_tag` - (Optional) The Service Tag used for this IP Restriction.

* `virtual_network_subnet_id` - (Optional) The Virtual Network Subnet ID used for this IP Restriction.

~> **NOTE:** One and only one of `ip_address`, `service_tag` or `virtual_network_subnet_id` must be specified.

---

A `site_config` block supports the following:

* `always_on` - (Optional) If this Windows Web App is Always On enabled. Defaults to `false`.

* `api_management_config_id` - (Optional) The ID of the APIM configuration for this Windows Web App.

* `app_command_line` - (Optional) The App command line to launch.

* `application_stack` - (Optional) A `application_stack` block as defined above.

* `auto_heal` - (Optional) Should Auto heal rules be enabled. Required with `auto_heal_setting`.

* `auto_heal_setting` - (Optional) A `auto_heal_setting` block as defined above. Required with `auto_heal`.

* `auto_swap_slot_name` - (Optional) The Windows Web App Slot Name to automatically swap to when deployment to that slot is successfully completed.

* `cors` - (Optional) A `cors` block as defined above.

* `default_documents` - (Optional) Specifies a list of Default Documents for the Windows Web App.

* `ftps_state` - (Optional) The State of FTP / FTPS service. Possible values include: `AllAllowed`, `FtpsOnly`, `Disabled`.

~> **NOTE:** Azure defaults this value to `AllAllowed`, however, in the interests of security Terraform will default this to `Disabled` to ensure the user makes a conscious choice to enable it.

* `health_check_path` - (Optional) The path to the Health Check.

* `http2_enabled` - (Optional) Should the HTTP2 be enabled?

* `ip_restriction` - (Optional) One or more `ip_restriction` blocks as defined above.

* `load_balancing_mode` - (Optional) The Site load balancing. Possible values include: `WeightedRoundRobin`, `LeastRequests`, `LeastResponseTime`, `WeightedTotalTraffic`, `RequestHash`, `PerSiteRoundRobin`. Defaults to `LeastRequests` if omitted.

* `local_mysql` - (Optional) Use Local MySQL. Defaults to `false`.

* `managed_pipeline_mode` - (Optional) Managed pipeline mode. Possible values include: `Integrated`, `Classic`.

* `minimum_tls_version` - (Optional) The configures the minimum version of TLS required for SSL requests. Possible values include: `1.0`, `1.1`, and  `1.2`. Defaults to `1.2`. 

* `number_of_workers` - (Optional) The number of Workers for this Windows App Service. 

* `remote_debugging` - (Optional) Should Remote Debugging be enabled. Defaults to `false`.

* `remote_debugging_version` - (Optional) The Remote Debugging Version. Possible values include `VS2017` and `VS2019`

* `scm_ip_restriction` - (Optional) One or more `scm_ip_restriction` blocks as defined above.

* `scm_minimum_tls_version` - (Optional) The configures the minimum version of TLS required for SSL requests to the SCM site Possible values include: `1.0`, `1.1`, and  `1.2`. Defaults to `1.2`.

* `scm_use_main_ip_restriction` - (Optional) Should the Windows Web App `ip_restriction` configuration be used for the SCM also.

* `32_bit_worker` - (Optional) Should the Windows Web App use a 32-bit worker.

* `virtual_application` - (Optional) One or more `virtual_application` blocks as defined below.

* `websockets` - (Optional) Should Web Sockets be enabled. Defaults to `false`. 

---

A `slow_request` block supports the following:

* `count` - (Required) The number of Slow Requests in the time `interval` to trigger this rule.

* `interval` - (Required) The time interval in the form `hh:mm:ss`.

* `time_taken` - (Required) The threshold of time passed to qualify as a Slow Request in `hh:mm:ss`.

* `path` - (Optional) The path for which this slow request rule applies.

---

A `status_code` block supports the following:

* `count` - (Required) The number of occurrences of the defined `status_code` in the specified `interval` on which to trigger this rule.

* `interval` - (Required) The time interval in the form `hh:mm:ss`.

* `status_code_range` - (Required) The status code for this rule, accepts single status codes and status code ranges. e.g. `500` or `400-499`. Possible values are integers between `101` and `599`

* `path` - (Optional) The path to which this rule status code applies. 

* `sub_status` - (Optional) The Request Sub Status of the Status Code.

* `win32_status` - (Optional) The Win32 Status Code of the Request.

---

A `storage_account` block supports the following:

* `access_key` - (Required) The Access key for the storage account.

* `account_name` - (Required) The Name of the Storage Account.

* `name` - (Required) The name which should be used for this TODO.

* `share_name` - (Required) The Name of the File Share or Container Name for Blob storage.

* `type` - (Required) The Azure Storage Type. Possible values include `AzureFiles` and `AzureBlob`

* `mount_path` - (Optional) The path at which to mount the storage share.

---

A `trigger` block supports the following:

* `private_memory_kb` - (Optional) The amount of Private Memory to be consumed for this rule to trigger. Possible values are between `102400` and  `13631488`.

* `requests` - (Optional) A `requests` block as defined above.

* `slow_request` - (Optional) One or more `slow_request` blocks as defined above.

* `status_code` - (Optional) One or more `status_code` blocks as defined above.

---

A `twitter` block supports the following:

* `consumer_key` - (Required) The OAuth 1.0a consumer key of the Twitter application used for sign-in. 
  
* `consumer_secret` - (Optional) The OAuth 1.0a consumer secret of the Twitter application used for sign-in. Cannot be specified with `consumer_secret_setting_name`.
  
* `consumer_secret_setting_name` - (Optional) The app setting name that contains the OAuth 1.0a consumer secret of the Twitter application used for sign-in. Cannot be specified with `consumer_secret`.

---

A `virtual_application` block supports the following:

* `physical_path` - (Required) The physical path for the Virtual Application.

* `preload` - (Required) Should pre-loading be enabled. Defaults to `false`.

* `virtual_directory` - (Optional) One or more `virtual_directory` blocks as defined below.

* `virtual_path` - (Required) The Virtual Path for the Virtual Application.

---

A `virtual_directory` block supports the following:

* `physical_path` - (Optional) The physical path for the Virtual Application.

* `virtual_path` - (Optional) The Virtual Path for the Virtual Application.

## Attributes Reference

In addition to the Arguments listed above - the following Attributes are exported: 

* `id` - The ID of the Windows Web App.

* `custom_domain_verification_id` - The ID of the TODO.

* `default_hostname` - The default hostname of the Windows Web App.

* `kind` - The Kind value for this Windows Web App. 

* `outbound_ip_address_list` - A list of outbound IP addresses - such as `["52.23.25.3", "52.143.43.12"]`

* `outbound_ip_addresses` - A comma separated list of outbound IP addresses - such as `52.23.25.3,52.143.43.12`.

* `possible_outbound_ip_address_list` - A `possible_outbound_ip_address_list` block as defined below.

* `possible_outbound_ip_addresses` - A comma separated list of outbound IP addresses - such as `52.23.25.3,52.143.43.12,52.143.43.17` - not all of which are necessarily in use. Superset of `outbound_ip_addresses`.

* `site_credential` - A `site_credential` block as defined below.

---

A `site_credential` block exports the following:

* `name` - The Site Credentials Username used for publishing.

* `password` - The Site Credentials Password used for publishing.

## Timeouts

The `timeouts` block allows you to specify [timeouts](https://www.terraform.io/docs/configuration/resources.html#timeouts) for certain actions:

* `create` - (Defaults to 30 minutes) Used when creating the Windows Web App.
* `read` - (Defaults to 5 minutes) Used when retrieving the Windows Web App.
* `update` - (Defaults to 30 minutes) Used when updating the Windows Web App.
* `delete` - (Defaults to 30 minutes) Used when deleting the Windows Web App.

## Import

Windows Web Apps can be imported using the `resource id`, e.g.

```shell
terraform import azurerm_windows_web_app.example /subscriptions/12345678-1234-9876-4563-123456789012/resourceGroups/resGroup1/providers/Microsoft.Web/sites/site1
```