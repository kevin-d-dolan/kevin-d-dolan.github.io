Connect-MgGraph -Scopes "DeviceManagementManagedDevices.Read.All"

$Devices = Get-MgDeviceManagementManagedDevice -All |
    Where-Object {$_.OperatingSystem -eq "Windows"}

$Devices |
    Select-Object `
        DeviceName,
        UserPrincipalName,
        ComplianceState,
        AzureADRegistered,
        LastSyncDateTime,
        IsEncrypted |
    Export-Csv ".\BitLocker-Status.csv" -NoTypeInformation

Write-Host "Report exported to BitLocker-Status.csv"
