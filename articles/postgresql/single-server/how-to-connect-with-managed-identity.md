---
title: Connect with Managed Identity - Azure Database for PostgreSQL - Single Server
description: Learn about how to connect and authenticate using Managed Identity for authentication with Azure Database for PostgreSQL
ms.service: postgresql
ms.subservice: single-server
ms.topic: how-to
ms.author: sunila
author: sunilagarwal
ms.custom: devx-track-csharp, devx-track-azurecli
ms.date: 06/24/2022
---

# Connect with Managed Identity to Azure Database for PostgreSQL

[!INCLUDE [applies-to-postgresql-single-server](../includes/applies-to-postgresql-single-server.md)]

You can use both system-assigned and user-assigned managed identities to authenticate to Azure Database for PostgreSQL. This article shows you how to use a system-assigned managed identity for an Azure Virtual Machine (VM) to access an Azure Database for PostgreSQL server. Managed Identities are automatically managed by Azure and enable you to authenticate to services that support Azure AD authentication, without needing to insert credentials into your code.

You learn how to:
- Grant your VM access to an Azure Database for PostgreSQL server
- Create a user in the database that represents the VM's system-assigned identity
- Get an access token using the VM identity and use it to query an Azure Database for PostgreSQL server
- Implement the token retrieval in a C# example application

## Prerequisites

- If you're not familiar with the managed identities for Azure resources feature, see this [overview](../../../articles/active-directory/managed-identities-azure-resources/overview.md). If you don't have an Azure account, [sign up for a free account](https://azure.microsoft.com/free/) before you continue.
- To do the required resource creation and role management, your account needs "Owner" permissions at the appropriate scope (your subscription or resource group). If you need assistance with role assignment, see [Assign Azure roles to manage access to your Azure subscription resources](../../../articles/role-based-access-control/role-assignments-portal.md).
- You need an Azure VM (for example running Ubuntu Linux) that you'd like to use for access your database using Managed Identity
- You need an Azure Database for PostgreSQL database server that has [Azure AD authentication](how-to-configure-sign-in-azure-ad-authentication.md) configured
- To follow the C# example, first complete the guide how to [Connect with C#](connect-csharp.md)

## Creating a system-assigned managed identity for your VM

Use [az vm identity assign](/cli/azure/vm/identity/) with the `identity assign` command enable the system-assigned identity to an existing VM:

```azurecli-interactive
az vm identity assign -g myResourceGroup -n myVm
```

Retrieve the application ID for the system-assigned managed identity, which you'll need in the next few steps:

```azurecli
# Get the client ID (application ID) of the system-assigned managed identity

[!INCLUDE [applies-to-postgresql-single-server](../includes/applies-to-postgresql-single-server.md)]


az ad sp list --display-name vm-name --query [*].appId --out tsv
```

## Creating a PostgreSQL user for your Managed Identity

Now, connect as the Azure AD administrator user to your PostgreSQL database, and run the following SQL statements, replacing `CLIENT_ID` with the client ID you retrieved for your system-assigned managed identity:

```sql
SET aad_validate_oids_in_tenant = off;
CREATE ROLE myuser WITH LOGIN PASSWORD 'CLIENT_ID' IN ROLE azure_ad_user;
```

The managed identity now has access when authenticating with the username `myuser` (replace with a name of your choice).

## Retrieving the access token from Azure Instance Metadata service

Your application can now retrieve an access token from the Azure Instance Metadata service and use it for authenticating with the database.

This token retrieval is done by making an HTTP request to `http://169.254.169.254/metadata/identity/oauth2/token` and passing the following parameters:

* `api-version` = `2018-02-01`
* `resource` = `https://ossrdbms-aad.database.windows.net`
* `client_id` = `CLIENT_ID` (that you retrieved earlier)

You'll get back a JSON result that contains an `access_token` field - this long text value is the Managed Identity access token, that you should use as the password when connecting to the database.

For testing purposes, you can run the following commands in your shell. Note you need `curl`, `jq`, and the `psql` client installed.

```bash
# Retrieve the access token

[!INCLUDE [applies-to-postgresql-single-server](../includes/applies-to-postgresql-single-server.md)]


export PGPASSWORD=`curl -s 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fossrdbms-aad.database.windows.net&client_id=CLIENT_ID' -H Metadata:true | jq -r .access_token`

# Connect to the database

[!INCLUDE [applies-to-postgresql-single-server](../includes/applies-to-postgresql-single-server.md)]


psql -h SERVER --user USER@SERVER DBNAME
```

You are now connected to the database you've configured earlier.

## Connecting using Managed Identity in C#

This section shows how to get an access token using the VM's user-assigned managed identity and use it to call Azure Database for PostgreSQL. Azure Database for PostgreSQL natively supports Azure AD authentication, so it can directly accept access tokens obtained using managed identities for Azure resources. When creating a connection to PostgreSQL, you pass the access token in the password field.

Here's a .NET code example of opening a connection to PostgreSQL using an access token. This code must run on the VM to use the system-assigned managed identity to obtain an access token from Azure AD. Replace the values of HOST, USER, DATABASE, and CLIENT_ID.

```csharp
using System;
using System.Net;
using System.IO;
using System.Collections;
using System.Collections.Generic;
using System.Text.Json;
using System.Text.Json.Serialization;
using Npgsql;
using Azure.Identity;

namespace Driver
{
    class Script
    {
        // Obtain connection string information from the portal for use in the following variables
        private static string Host = "HOST";
        private static string User = "USER";
        private static string Database = "DATABASE";

        static async Task Main(string[] args)
        {
            //
            // Get an access token for PostgreSQL.
            //
            Console.Out.WriteLine("Getting access token from Azure AD...");

            // Azure AD resource ID for Azure Database for PostgreSQL is https://ossrdbms-aad.database.windows.net/
            string accessToken = null;

            try
            {
                // Call managed identities for Azure resources endpoint.
                var sqlServerTokenProvider = new DefaultAzureCredential();
                accessToken = (await sqlServerTokenProvider.GetTokenAsync(
                    new Azure.Core.TokenRequestContext(scopes: new string[] { "https://ossrdbms-aad.database.windows.net/.default" }) { })).Token;

            }
            catch (Exception e)
            {
                Console.Out.WriteLine("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                System.Environment.Exit(1);
            }

            //
            // Open a connection to the PostgreSQL server using the access token.
            //
            string connString =
                String.Format(
                    "Server={0}; User Id={1}; Database={2}; Port={3}; Password={4}; SSLMode=Prefer",
                    Host,
                    User,
                    Database,
                    5432,
                    accessToken);

            using (var conn = new NpgsqlConnection(connString))
            {
                Console.Out.WriteLine("Opening connection using access token...");
                conn.Open();

                using (var command = new NpgsqlCommand("SELECT version()", conn))
                {

                    var reader = command.ExecuteReader();
                    while (reader.Read())
                    {
                        Console.WriteLine("\nConnected!\n\nPostgres version: {0}", reader.GetString(0));
                    }
                }
            }
        }
    }
}
```

When run, this command will give an output like this:

```
Getting access token from Azure AD...
Opening connection using access token...

Connected!

Postgres version: PostgreSQL 11.11, compiled by Visual C++ build 1800, 64-bit
```

## Next steps

* Review the overall concepts for [Azure Active Directory authentication with Azure Database for PostgreSQL](concepts-azure-ad-authentication.md)
