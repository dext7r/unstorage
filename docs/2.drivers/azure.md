----
icon: mdi:microsoft-azure
---

# Azure

## Azure App Configuration

将数据存储在 Azure App Configuration 的键值存储中。

### 使用方法

::note{to="https://learn.microsoft.com/en-us/azure/azure-app-configuration/overview"}
了解有关 Azure App Configuration 的更多信息。
::

该驱动程序使用配置存储作为键值存储。它将 `key` 用作名称，`value` 用作内容。您还可以使用标签来区分不同的环境（开发、生产等），并使用前缀来区分不同的应用程序（app01、app02 等）。

要使用它，您需要在项目中安装 `@azure/app-configuration` 和 `@azure/identity`：

:pm-install{name="@azure/app-configuration @azure/identity"}

使用示例：

```js
import { createStorage } from "unstorage";
import azureAppConfiguration from "unstorage/drivers/azure-app-configuration";

const storage = createStorage({
  driver: azureAppConfiguration({
    appConfigName: "unstoragetest",
    label: "dev",
    prefix: "app01",
  }),
});
```

**身份验证：**

该驱动程序支持以下身份验证方法：

- **`DefaultAzureCredential`**：这是推荐的身份验证方式。它会使用托管身份或环境变量来验证请求。它还将在本地环境中通过尝试使用 Azure CLI 或 Azure PowerShell 进行身份验证。 <br>
  ⚠️ 确保您的托管身份或个人帐户具有分配的 `App Configuration Data Owner` 角色，即使您已在应用程序配置资源上是 `Contributor` 或 `Owner`。
- **`connectionString`**：应用程序配置连接字符串。建议在生产中不使用。

**选项：**

- `appConfigName`：应用程序配置资源的名称。
- `endpoint`：应用程序配置资源的终结点。
- `connectionString`：应用程序配置资源的连接字符串。
- `prefix`：键的可选前缀。这可以用于在同一 Azure App Configuration 实例中隔离来自不同应用程序的键。例如， "app01" 生成的键类似于 "app01:foo" 和 "app01:bar"。
- `label`：键的可选标签。如果未提供，将创建并列出所有没有标签的键。这可以用于在同一 Azure App Configuration 实例中隔离来自不同环境的键。例如，"dev" 生成的键类似于带有标签 "dev" 的 "foo" 和 "bar"。

## Azure Cosmos DB

将数据存储在 Azure Cosmos DB NoSQL API 文档中。

### 使用方法

::note{to="https://azure.microsoft.com/en-us/services/cosmos-db/"}
了解更多 Azure Cosmos DB 的信息。
::

该驱动程序将 KV 信息存储在 NoSQL API Cosmos DB 集合中作为文档。它使用 `id` 字段作为键，并将 `value` 和 `modified` 字段添加到文档中。

要使用它，您需要在项目中安装 `@azure/cosmos` 和 `@azure/identity`：

:pm-install{name="@azure/cosmos @azure/identity"}

使用示例：

```js
import { createStorage } from "unstorage";
import azureCosmos from "unstorage/drivers/azure-cosmos";

const storage = createStorage({
  driver: azureCosmos({
    endpoint: "ENDPOINT",
    accountKey: "ACCOUNT_KEY",
  }),
});
```

**身份验证：**

- **`DefaultAzureCredential`**：这是推荐的身份验证方式。它会使用托管身份或环境变量来验证请求。它还将在本地环境中通过尝试使用 Azure CLI 或 Azure PowerShell 进行身份验证。 <br>
  ⚠️ 确保您的托管身份或个人帐户至少分配了 `Cosmos DB Built-in Data Contributor` 角色。如果您已是该资源的 `Contributor` 或 `Owner`，这也应该足够，但这并未实现最小权限模型。
- **`accountKey`**：CosmosDB 账户密钥。如果未提供，驱动程序将使用 DefaultAzureCredential（推荐）。

**选项：**

- **`endpoint`**（必需）：CosmosDB 终结点，格式为 `https://<account>.documents.azure.com:443/`。
- `accountKey`：CosmosDB 账户密钥。如果未提供，驱动程序将使用 DefaultAzureCredential（推荐）。
- `databaseName`：要使用的数据库名称。默认为 `unstorage`。
- `containerName`：要使用的容器名称。默认为 `unstorage`。

## Azure Key Vault

将数据存储在 Azure Key Vault 秘密中。

### 使用方法

::note{to="https://docs.microsoft.com/en-us/azure/key-vault/secrets/about-secrets"}
了解更多有关 Azure Key Vault 秘密的信息。
::

该驱动程序通过将键用作秘密 ID，将值用作秘密内容，来将 KV 信息存储在 Azure Key Vault 秘密中。
请注意，密钥保管库秘密的访问时间并不是最快的，并且并未设计用于高吞吐量。您还必须禁用密钥保管库的清除保护才能删除秘密。此实现会在删除时删除并清除秘密，以避免与软删除发生冲突。

⚠️ 请注意，该驱动程序以编码方式存储您的 `key:value` 对中的键，以避免与秘密的命名要求发生冲突。这意味着，只要密钥不存在以相同方式编码，您将无法访问手动创建的（在 unstorage 外部）密钥保管库中的秘密。

要使用它，您需要在项目中安装 `@azure/keyvault-secrets` 和 `@azure/identity`：

:pm-install{name="@azure/keyvault-secrets @azure/identity"}

使用示例：

```js
import { createStorage } from "unstorage";
import azureKeyVault from "unstorage/drivers/azure-key-vault";

const storage = createStorage({
  driver: azureKeyVault({
    vaultName: "testunstoragevault",
  }),
});
```

**身份验证：**

该驱动程序支持以下身份验证方法：

- **`DefaultAzureCredential`**：这是推荐的身份验证方式。它会使用托管身份或环境变量来验证请求。它还将在本地环境中通过尝试使用 Azure CLI 或 Azure PowerShell 进行身份验证。

⚠️ 确保您的托管身份或个人账户具有分配的 `Key Vault Secrets Officer`（或 `Key Vault Secrets User` 仅限只读）RBAC 角色，或是一个访问策略的成员，该策略授予 `Get`、`List`、`Set`、`Delete` 和 `Purge` 秘密权限。

**选项：**

- **`vaultName`**（必需）：要使用的密钥保管库的名称。
- `serviceVersion`：要使用的 Azure Key Vault 服务版本。默认为 7.3。
- `pageSize`：每个请求要检索的条目数。影响 getKeys() 和 clear() 的性能。最大值为 25。

## Azure Blob Storage

将数据存储在 Azure blob 存储中。

### 使用方法

::note{to="https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/storage/storage-blob"}
了解关于 Azure blob 存储的更多信息。
::

该驱动程序将 KV 信息存储在 Azure blob 存储 blob 中。所有条目使用相同的容器。每个条目存储在一个单独的 blob 中，键作为 blob 名称，值作为 blob 内容。

要使用它，您需要在项目中安装 `@azure/storage-blob` 和 `@azure/identity`：

:pm-install{name="@azure/storage-blob @azure/identity"}

请确保您要使用的容器在您的存储账户中存在。

```js
import { createStorage } from "unstorage";
import azureStorageBlobDriver from "unstorage/drivers/azure-storage-blob";

const storage = createStorage({
  driver: azureStorageBlobDriver({
    accountName: "myazurestorageaccount",
  }),
});
```

**身份验证：**

该驱动程序支持以下身份验证方法：

- **`DefaultAzureCredential`**：这是推荐的身份验证方式。它会使用托管身份或环境变量来验证请求。它还将在本地环境中通过尝试使用 Azure CLI 或 Azure PowerShell 进行身份验证。 <br>
  ⚠️ 确保您的托管身份或个人帐户具有分配的 `Storage Blob Data Contributor` 角色，即使您已在存储帐户上是 `Contributor` 或 `Owner`。
- **`AzureNamedKeyCredential`**（仅在 Node.js 运行时可用）：这将使用 `accountName` 和 `accountKey` 来验证请求。
- **`AzureSASCredential`**：这将使用 `accountName` 和 `sasToken` 来验证请求。
- **连接字符串**（仅在 Node.js 运行时可用）：这将使用 `connectionString` 来验证请求。不推荐使用，因为它将以明文形式暴露您的帐户密钥。

**选项：**

- **`accountName`**（必需）：您的存储帐户的名称。
- `containerName`：要使用的 blob 容器的名称。默认为 `unstorage`。
- `accountKey`：用于身份验证的账户密钥。仅在使用 `AzureNamedKeyCredential` 时需要。
- `sasKey`：用于身份验证的 SAS 令牌。仅在使用 `AzureSASCredential` 时需要。
- `connectionString`：存储帐户的连接字符串。`accountKey` 和 `sasKey` 具有优先权。

## Azure Table Storage

将数据存储在 Azure 表存储中。

### 使用方法

::note{to="https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/tables/data-tables"}
了解有关 Azure 表存储的更多信息。
::

::warning
该驱动程序当前不兼容于 Cloudflare Workers 或 Vercel Edge Functions 等边缘工作者。未来可能会有基于 http 的驱动程序。
::

将数据存储在 [data-tables]() 中。

该驱动程序将 KV 信息存储在 Azure 表存储中。所有键使用相同的分区键，字段 `unstorageValue` 用于存储值。

要使用它，您需要在项目中安装 `@azure/data-table` 和 `@azure/identity`：

:pm-install{name="@azure/data-table @azure/identity"}

请确保您要使用的表在您的存储帐户中存在。

```js
import { createStorage } from "unstorage";
import azureStorageTableDriver from "unstorage/drivers/azure-storage-table";

const storage = createStorage({
  driver: azureStorageTableDriver({
    accountName: "myazurestorageaccount",
  }),
});
```

**身份验证：**

该驱动程序支持以下身份验证方法：

- **`DefaultAzureCredential`**：这是推荐的身份验证方式。它会使用托管身份或环境变量来验证请求。它还将在本地环境中通过尝试使用 Azure CLI 或 Azure PowerShell 进行身份验证。

  ⚠️ 确保您的托管身份或个人帐户具有分配的 `Storage Table Data Contributor` 角色，即使您已在存储帐户中是 `Contributor` 或 `Owner`。

- **`AzureNamedKeyCredential`**（仅在 Node.js 运行时可用）：这将使用 `accountName` 和 `accountKey` 验证请求。
- **`AzureSASCredential`**：这将使用 `accountName` 和 `sasToken` 验证请求。
- **连接字符串**（仅在 Node.js 运行时可用）：这将使用 `connectionString` 验证请求。不推荐使用，因为它将以明文形式暴露您的帐户密钥。

**选项：**

- **`accountName`**（必需）：您的存储帐户的名称。
- `tableName`：要使用的表的名称。默认为 `unstorage`。
- `partitionKey`：要使用的分区键。默认为 `unstorage`。
- `accountKey`：用于身份验证的帐户密钥。仅在使用 `AzureNamedKeyCredential` 时需要。