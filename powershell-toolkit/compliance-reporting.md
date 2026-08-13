Connect-MgGraph -Scopes "DeviceManagementManagedDevices.Read.All"

$Devices = Get-MgDeviceManagementManagedDevice -All

$Devices |
Select-Object `
    DeviceName,
    UserPrincipalName,
    OperatingSystem,
    ComplianceState,
    LastSyncDateTime |
Export-Csv ".\ComplianceReport.csv" -NoTypeInformation

Write-Host "Report exported to ComplianceReport.csv"
