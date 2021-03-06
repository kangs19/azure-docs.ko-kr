---
title: .NET에서 Blob 스냅샷 만들기 및 관리
titleSuffix: Azure Storage
description: Blob의 읽기 전용 스냅숏을 만들어 지정 된 시간에 blob 데이터를 백업 하는 방법에 대해 알아봅니다.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 04/02/2020
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: be3d06d8ea493ab7c246ace5c49d5e5bc0036108
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: ko-KR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84463538"
---
# <a name="create-and-manage-a-blob-snapshot-in-net"></a>.NET에서 Blob 스냅샷 만들기 및 관리

스냅샷은 특정 시점에 생성된 Blob의 읽기 전용 버전입니다. 이 문서에서는 [.net 용 Azure Storage 클라이언트 라이브러리](/dotnet/api/overview/azure/storage?view=azure-dotnet)를 사용 하 여 blob 스냅숏을 만들고 관리 하는 방법을 보여 줍니다.

Azure Storage의 blob 스냅숏에 대 한 자세한 내용은 [.net에서 blob 스냅숏 만들기 및 관리](snapshots-overview.md)를 참조 하세요.

## <a name="create-a-snapshot"></a>스냅샷 만들기

# <a name="net-version-12x"></a>[.NET 버전 2.x](#tab/v12)

.NET 용 Azure Storage 클라이언트 라이브러리의 버전 2.x를 사용 하 여 블록 blob의 스냅숏을 만들려면 다음 방법 중 하나를 사용 합니다.

- [고 icloudblob.createsnapshot](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshot)
- [CreateSnapshotAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.createsnapshotasync)

다음 코드 예제에서는 버전 12. x를 사용 하 여 스냅숏을 만드는 방법을 보여 줍니다. Azure AD 자격 증명을 사용 하 여 서비스에 대 한 요청을 인증 하는 데 사용 되는 [Azure id](https://www.nuget.org/packages/azure.identity) 라이브러리에 대 한 참조를 포함 합니다.

```csharp
private static async Task CreateBlockBlobSnapshot(string accountName, string containerName, string blobName, Stream data)
{
    const string blobServiceEndpointSuffix = ".blob.core.windows.net";
    Uri containerUri = new Uri("https://" + accountName + blobServiceEndpointSuffix + "/" + containerName);

    // Get a container client object and create the container.
    BlobContainerClient containerClient = new BlobContainerClient(containerUri,
        new DefaultAzureCredential());
    await containerClient.CreateIfNotExistsAsync();

    // Get a blob client object.
    BlobClient blobClient = containerClient.GetBlobClient(blobName);

    try
    {
        // Upload text to create a block blob.
        await blobClient.UploadAsync(data);

        // Add blob metadata.
        IDictionary<string, string> metadata = new Dictionary<string, string>
        {
            { "ApproxBlobCreatedDate", DateTime.UtcNow.ToString() },
            { "FileType", "text" }
        };
        await blobClient.SetMetadataAsync(metadata);

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        await blobClient.CreateSnapshotAsync();
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

# <a name="net-version-11x"></a>[.NET 버전 11.x](#tab/v11)

.NET 용 Azure Storage 클라이언트 라이브러리의 버전 11. x를 사용 하 여 블록 blob의 스냅숏을 만들려면 다음 방법 중 하나를 사용 합니다.

- [고 icloudblob.createsnapshot](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshot)
- [CreateSnapshotAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblockblob.createsnapshotasync)

다음 코드 예제에서는 버전 17.x를 사용 하 여 스냅숏을 만드는 방법을 보여 줍니다. 이 예제에서는 만들 때 스냅샷에 대한 추가 메타데이터를 지정합니다.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // You can specify metadata at the time that the snapshot is created.
        // If no metadata is specified, then the blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
        Console.WriteLine(snapshot.SnapshotQualifiedStorageUri.PrimaryUri);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

---

## <a name="delete-snapshots"></a>스냅샷 삭제

Blob을 삭제 하려면 먼저 해당 blob의 모든 스냅숏을 삭제 해야 합니다. 스냅샷을 개별적으로 삭제하거나 원본 Blob를 삭제할 때 모든 스냅샷을 삭제되도록 지정할 수 있습니다. 스냅샷이 있는 Blob을 삭제하려고 하면 오류가 발생합니다.

# <a name="net-version-12x"></a>[.NET 버전 2.x](#tab/v12)

.NET 용 Azure Storage 클라이언트 라이브러리의 버전 12. x를 사용 하 여 blob 및 해당 스냅숏을 삭제 하려면 다음 방법 중 하나를 사용 하 고 [DeleteSnapshotsOption](/dotnet/api/azure.storage.blobs.models.deletesnapshotsoption) 열거형을 포함 합니다.

- [삭제](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.delete)
- [DeleteAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteasync)
- [DeleteIfExists](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/azure.storage.blobs.specialized.blobbaseclient.deleteifexistsasync)

다음 코드 예제에서는 .NET에서 blob 및 해당 스냅숏을 삭제 하는 방법을 보여 줍니다 `blobClient` . 여기서은 [blobclient](/dotnet/api/azure.storage.blobs.blobclient)형식의 개체입니다.

```csharp
await blobClient.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, default);
```

# <a name="net-version-11x"></a>[.NET 버전 11.x](#tab/v11)

.NET 용 Azure Storage 클라이언트 라이브러리의 버전 11. x를 사용 하 여 blob 및 해당 스냅숏을 삭제 하려면 다음 blob 삭제 방법 중 하나를 사용 하 고 [DeleteSnapshotsOption](/dotnet/api/microsoft.azure.storage.blob.deletesnapshotsoption) 열거형을 포함 합니다.

- [삭제](/dotnet/api/microsoft.azure.storage.blob.cloudblob.delete)
- [DeleteAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblob.deleteifexistsasync)

다음 코드 예제에서는 .NET에서 blob 및 해당 스냅숏을 삭제 하는 방법을 보여 줍니다 `blockBlob` . 여기서은 [CloudBlockBlob] [dotnet_CloudBlockBlob] 형식의 개체입니다.

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

---

## <a name="next-steps"></a>다음 단계

- [Blob 스냅숏](snapshots-overview.md)
- [Blob 버전 (미리 보기)](versioning-overview.md)
- [Blob에 대 한 일시 삭제](storage-blob-soft-delete.md)