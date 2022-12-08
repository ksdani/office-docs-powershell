---
external help file: Microsoft.Exchange.TransportMailflow-Help.xml
online version: https://learn.microsoft.com/powershell/module/exchange/set-reportsubmissionpolicy
applicable: Exchange Online
title: Set-ReportSubmissionPolicy
schema: 2.0.0
author: chrisda
ms.author: chrisda
ms.reviewer:
---

# Set-ReportSubmissionPolicy

## SYNOPSIS
This cmdlet is available only in the cloud-based service.

Use the Set-ReportSubmissionPolicy cmdlet to modify the report submission policy in your cloud-based organization.

For information about the parameter sets in the Syntax section below, see [Exchange cmdlet syntax](https://learn.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## SYNTAX

```
Set-ReportSubmissionPolicy [-Identity] <ReportSubmissionPolicyIdParameter>
 [-Confirm]
 [-DisableQuarantineReportingOption <Boolean>]
 [-DisableUserSubmissionOptions <Boolean>]
 [-EnableCustomNotificationSender <Boolean>]
 [-EnableCustomizedMsg <Boolean>]
 [-EnableOrganizationBranding <Boolean>]
 [-EnableReportToMicrosoft <Boolean>]
 [-EnableThirdPartyAddress <Boolean>]
 [-EnableUserEmailNotification <Boolean>]
 [-JunkReviewResultMessage <String>]
 [-NotJunkReviewResultMessage <String>]
 [-NotificationFooterMessage <String>]
 [-NotificationSenderAddress <MultiValuedProperty>]
 [-OnlyShowPhishingDisclaimer <Boolean>]
 [-PhishingReviewResultMessage <String>]
 [-PostSubmitMessage <String>]
 [-PostSubmitMessageForJunk <String>]
 [-PostSubmitMessageForNotJunk <String>]
 [-PostSubmitMessageForPhishing <String>]
 [-PostSubmitMessageTitle <String>]
 [-PostSubmitMessageTitleForJunk <String>]
 [-PostSubmitMessageTitleForNotJunk <String>]
 [-PostSubmitMessageTitleForPhishing <String>]
 [-PreSubmitMessage <String>]
 [-PreSubmitMessageForJunk <String>]
 [-PreSubmitMessageForNotJunk <String>]
 [-PreSubmitMessageForPhishing <String>]
 [-PreSubmitMessageTitle <String>]
 [-PreSubmitMessageTitleForJunk <String>]
 [-PreSubmitMessageTitleForNotJunk <String>]
 [-PreSubmitMessageTitleForPhishing <String>]
 [-ReportJunkAddresses <MultiValuedProperty>]
 [-ReportJunkToCustomizedAddress <Boolean>]
 [-ReportNotJunkAddresses <MultiValuedProperty>]
 [-ReportNotJunkToCustomizedAddress <Boolean>]
 [-ReportPhishAddresses <MultiValuedProperty>]
 [-ReportPhishToCustomizedAddress <Boolean>]
 [-ThirdPartyReportAddresses <MultiValuedProperty>]
 [-UserSubmissionOptions <Int32>]
 [-UserSubmissionOptionsMessage <String>]
 [-WhatIf] [<CommonParameters>]
```

## DESCRIPTION
The report submission policy controls most of the settings for user submissions in the Microsoft 365 Defender portal at <https://security.microsoft.com/userSubmissionsReportMessage>.

The report submission rule (the SentTo parameter \*-ReportSubmissionRule cmdlets) controls the email address of the user submissions mailbox where user reported messages are sent.

When you set the email address of the user submissions mailbox in the Microsoft 365 Defender portal at <https://security.microsoft.com/userSubmissionsReportMessage>, the same email address is also set in the following parameters in the \*-ReportSubmissionPolicy cmdlets:

- Microsoft integrated reporting: The ReportJunkAddresses, ReportNotJunkAddresses, and ReportPhishAddresses parameters.
- Third-party tools: The ThirdPartyReportAddresses parameter.

Although it's not absolutely required, it makes sense to keep the email address consistent in the related parameters in the \*-ReportSubmissionPolicy and \*-ReportSubmissionRule cmdlets.

You need to be assigned permissions before you can run this cmdlet. Although this topic lists all parameters for the cmdlet, you may not have access to some parameters if they're not included in the permissions assigned to you. To find the permissions required to run any cmdlet or parameter in your organization, see [Find the permissions required to run any Exchange cmdlet](https://learn.microsoft.com/powershell/exchange/find-exchange-cmdlet-permissions).

## EXAMPLES

### Example 1
```powershell
Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $true -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $false -ReportJunkAddresses $null -ReportNotJunkToCustomizedAddress $false -ReportNotJunkAddresses $null -ReportPhishToCustomizedAddress $false -ReportPhishAddresses $null

Get-ReportSubmissionRule | Remove-ReportSubmissionRule
```

This example turns on the Microsoft integrated reporting experience, but allows users to report messages to Microsoft only. The user submissions mailbox is not used.

**Notes**:

- You can use this example (and the other examples) without concern for the existing values of the specified parameters. Depending on your current configuration, some of the parameters might already be set to the required values and therefore might not be required.
- If the report submission rule doesn't exist, you don't need to run the Remove-ReportSubmissionRule command.

### Example 2
```powershell
$usersub = "reportedmessages@contoso.com"

Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $true -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example turns on the Microsoft integrated reporting experience, allows users to report messages to Microsoft, and sends reported messages to the specified user submissions mailbox. The New-ReportSubmissionRule command is required only if you don't already have a report submission rule.

### Example 3
```powershell
$usersub = "userreportedmessages@fabrikam.com"

Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $false -ThirdPartyReportAddresses $null -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example turns on the Microsoft integrated reporting experience and sends reported messages to the specified user submissions mailbox only (users can't report messages to Microsoft). The New-ReportSubmissionRule command is required only if you don't already have a report submission rule.

### Example 4
```powershell
$usersub = "thirdpartyreporting@wingtiptoys.com"

Set-ReportSubmissionPolicy -Identity DefaultReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $true -ThirdPartyReportAddresses $usersub -ReportJunkToCustomizedAddress $false -ReportJunkAddresses $null -ReportNotJunkToCustomizedAddress $false -ReportNotJunkAddresses $null -ReportPhishToCustomizedAddress $false -ReportPhishAddresses $null

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example turns off the Microsoft integrated reporting experience. Reported messages are sent to the specified user submissions using third-party tools. The New-ReportSubmissionRule command is required only if you don't already have a report submission rule.

## PARAMETERS

### -Identity
The Identity parameter specifies the report submission policy that you want to modify. The only available policy is named DefaultReportSubmissionPolicy.

```yaml
Type: ReportSubmissionPolicyIdParameter
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Confirm
The Confirm switch specifies whether to show or hide the confirmation prompt. How this switch affects the cmdlet depends on if the cmdlet requires confirmation before proceeding.

- Destructive cmdlets (for example, Remove-\* cmdlets) have a built-in pause that forces you to acknowledge the command before proceeding. For these cmdlets, you can skip the confirmation prompt by using this exact syntax: -Confirm:$false.
- Most other cmdlets (for example, New-\* and Set-\* cmdlets) don't have a built-in pause. For these cmdlets, specifying the Confirm switch without a value introduces a pause that forces you acknowledge the command before proceeding.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableQuarantineReportingOption
The DisableQuarantineReportingOption parameter allows or prevents users from reporting messages in quarantine. Valid values are:

- $true: Users can't report quarantined messages from quarantine.
- $false: Users can report quarantined messages from quarantine. This is the default value.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableUserSubmissionOptions
The DisableUserSubmissionOptions parameter specifies whether users are allowed to choose if they want to report a message. Valid values are:

- $true: Users aren't allowed to choose if they want to report a message. You specify the one and only option that's available to them using the UserSubmissionOptions parameter.
- $false: Users are allowed to choose if they want to report a message. You specify the options that are available to them using the UserSubmissionOptions parameter. This is the default value.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableCustomNotificationSender
The EnableCustomNotificationSender parameter specifies whether a custom sender email address is used for notifications for user submitted messages. Valid values are:

- $true: Use a custom Microsoft 365 sender email address.
- $false: Use the default sender email address. This is the default value.

You specify the sender email address using the NotificationSenderAddress parameter.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableCustomizedMsg
The EnableCustomizedMsg parameter enables or disables the customized messages that are shown to users before and/or after they report messages. Valid values are:

- $true: Customized messages are enabled.
- $false: Customized messages are disabled. This is the default value.

You customize the messages using the following parameters:

- PostSubmitMessage
- PostSubmitMessageTitle
- PreSubmitMessage
- PreSubmitMessageTitle

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableOrganizationBranding
The EnableOrganizationBranding parameter specifies whether to show the company logo in the footer of email notifications after an admin reviews and marks messages that were reported as junk, not junk, or phishing. Valid values are:

- $true: Use the company logo in the footer text.
- $false: Don't use the company logo in the footer text.

You can customize the footer in notification messages using the NotificationFooterMessage parameter.

You can customize the sender email address of notifications using the NotificationSenderAddress parameter.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableReportToMicrosoft
The EnableReportToMicrosoft parameter specifies whether Microsoft integrated reporting experience is enabled or disabled. Valid values are $true or $false.

The value $true for this parameter has the following possible outcomes:

- If the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters are set to the value $false, the Microsoft integrated reporting experience is enabled, but users can only report messages to Microsoft.
- If the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters are set to the value $true, the Microsoft integrated reporting experience is enabled. User reported messages are sent to Microsoft and a user submissions mailbox. Use the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet to specify the email address of the user submissions mailbox.

The value $false for this parameter has the following possible outcomes:

- If the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters are set to the value $false and the EnableThirdPartyAddress parameter is set to the value $true, the Microsoft integrated reporting experience is disabled in favor of a third-party product. Use the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet to specify the email address of the user submissions mailbox.
- If the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters are set to the value $true and the EnableThirdPartyAddress parameter is set to the value $false, the Microsoft integrated reporting experience is enabled, but user reported messages are sent only to the user submissions mailbox. Use the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet to specify the email address of the user submissions mailbox.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableThirdPartyAddress
The EnableThirdPartyAddress parameter specifies whether you're using a third-party product instead of the Microsoft integrated reporting experience. Valid values are:

- $true: The Microsoft integrated reporting experience is disabled in favor of a third-party product for user submissions. Set the EnableReportToMicrosoft parameter to the value $false, and use the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet to specify the email address of the user submissions mailbox.
- $false: You're not using a third-party product for user submissions. Set the EnableReportToMicrosoft parameter to the value $true to enable the Microsoft integrated reporting experience.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -EnableUserEmailNotification
This parameter is reserved for internal Microsoft use.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -JunkReviewResultMessage
The JunkReviewResultMessage parameter specifies the custom text to use in email notifications after an admin reviews and marks messages that were reported as junk. If the value contains spaces, enclose the value in quotation marks (").

You can customize the footer in notification messages using the NotificationFooterMessage and EnableOrganizationBranding parameters.

You can customize the sender email address of notifications using the NotificationSenderAddress parameter.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotJunkReviewResultMessage
The NotJunkReviewResultMessage parameter specifies the custom text to use in email notifications after an admin reviews and marks messages that were reported as not junk. If the value contains spaces, enclose the value in quotation marks (").

You can customize the footer in notification messages using the NotificationFooterMessage and EnableOrganizationBranding parameters.

You can customize the sender email address of notifications using the NotificationSenderAddress parameter.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotificationFooterMessage
The NotificationFooterMessage parameter specifies the custom footer text to use in email notifications after an admin reviews and marks messages that were reported as junk, not junk, or phishing. If the value contains spaces, enclose the value in quotation marks.

You can use the EnableOrganizationBranding parameter to include your company logo in the message footer.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NotificationSenderAddress
The NotificationSenderAddress parameter specifies the sender email address to use in email notifications after an admin reviews and marks messages that were reported as junk, not junk, or phishing. The email address must be in Exchange Online.

This parameter is meaningful only when the EnableCustomNotificationSender parameter value is $true.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type:
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OnlyShowPhishingDisclaimer
The OnlyShowPhishingDisclaimer parameter specifies whether to show users the before reporting and after reporting messages only for messages that were reported as phishing. Valid values are:

- $true: Show the before reporting and after reporting messages only when users report messages as phishing. This is the default value.
- $false: Show the before reporting and after reporting messages when users report messages as junk, not junk, and phishing.

You specify the title and message body for the before reporting and after reporting messages using the following parameters:

- PreSubmitMessageTitle
- PreSubmitMessage
- PostSubmitMessageTitle
- PostSubmitMessage

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PhishingReviewResultMessage
The PhishingReviewResultMessage parameter specifies the custom text to use in email notifications after an admin reviews and marks messages that were reported as phishing. If the value contains spaces, enclose the value in quotation marks (").

You can customize the footer in notification messages using the NotificationFooterMessage and EnableOrganizationBranding parameters.

You can customize the sender email address of notifications using the NotificationSenderAddress parameter.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessage
The PostSubmitMessage parameter specifies the custom message body text to use in notifications after users report messages. If the value contains spaces, enclose the value in quotation marks (").

You specify the custom title for after reporting messages using the PostSubmitMessageTitle parameter.

This parameter is meaningful only if the EnableCustomizedMsg parameter value is $true.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageForJunk
Don't use this parameter. Use the PostSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageForNotJunk
Don't use this parameter. Use the PostSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageForPhishing
Don't use this parameter. Use the PostSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageTitle
The PostSubmitMessage parameter parameter specifies the custom title text to use in notifications after users report messages. If the value contains spaces, enclose the value in quotation marks (").

You specify the custom message body for after reporting messages using the PostSubmitMessage parameter.

This parameter is meaningful only if the EnableCustomizedMsg parameter value is $true.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageTitleForJunk
Don't use this parameter. Use the PostSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageTitleForNotJunk
Don't use this parameter. Use the PostSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PostSubmitMessageTitleForPhishing
Don't use this parameter. Use the PostSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessage
The PreSubmitMessage parameter specifies the custom message body text to use in notifications before users report messages. If the value contains spaces, enclose the value in quotation marks (").

You specify the custom title for after reporting messages using the PreSubmitMessageTitle parameter.

This parameter is meaningful only if the EnableCustomizedMsg parameter value is $true.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageForJunk
Don't use this parameter. Use the PreSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageForNotJunk
Don't use this parameter. Use the PreSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageForPhishing
Don't use this parameter. Use the PreSubmitMessage parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageTitle
The PreSubmitMessage parameter parameter specifies the custom title text to use in notifications before users report messages. If the value contains spaces, enclose the value in quotation marks (").

You specify the custom message body for before reporting messages using the PreSubmitMessage parameter.

This parameter is meaningful only if the EnableCustomizedMsg parameter value is $true.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageTitleForJunk
Don't use this parameter. Use the PreSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageTitleForNotJunk
Don't use this parameter. Use the PreSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PreSubmitMessageTitleForPhishing
Don't use this parameter. Use the PreSubmitMessageTitle parameter instead.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportJunkAddresses
**Note**: You don't need to use this parameter. You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet. To reduce confusion, you can set this parameter to the same value.

The ReportJunkAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters in the same command.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: MultiValuedProperty
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportJunkToCustomizedAddress
The ReportJunkToCustomizedAddress parameter specifies whether to send user reported messages to the user submissions mailbox in the Microsoft integrated reporting experience. Valid values are:

- $true: User reported messages are sent to the user submissions mailbox.
- $false: User reported messages are not sent to the user submissions mailbox.

You can't use this parameter by itself. You need to specify the same value for the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters in the same command.

You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

Use the EnableReportToMicrosoft parameter to specify whether user reported messages are also sent to Microsoft.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportNotJunkAddresses
**Note**: You don't need to use this parameter. You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet. To reduce confusion, you can set this parameter to the same value.

The ReportNotJunkAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters in the same command.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: MultiValuedProperty
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportNotJunkToCustomizedAddress
The ReportNotJunkToCustomizedAddress parameter specifies whether to send user reported messages to the user submissions mailbox in the Microsoft integrated reporting experience. Valid values are:

- $true: User reported messages are sent to the user submissions mailbox.
- $false: User reported messages are not sent to the user submissions mailbox.

You can't use this parameter by itself. You need to specify the same value for the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters in the same command.

You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

Use the EnableReportToMicrosoft parameter to specify whether user reported messages are also sent to Microsoft.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportPhishAddresses
**Note**: You don't need to use this parameter. You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet. To reduce confusion, you can set this parameter to the same value.

The ReportPhishAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters in the same command.

This parameter is meaningful only when the ReportPhishToCustomizedAddress parameter value is $true.

```yaml
Type: MultiValuedProperty
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReportPhishToCustomizedAddress
The ReportPhishToCustomizedAddress parameter specifies whether to send user reported messages to the user submissions mailbox in the Microsoft integrated reporting experience. Valid values are:

- $true: User reported messages are sent to the user submissions mailbox.
- $false: User reported messages are not sent to the user submissions mailbox.

You can't use this parameter by itself. You need to specify the same value for the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters in the same command.

You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

Use the EnableReportToMicrosoft parameter to specify whether user reported messages are also sent to Microsoft.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Boolean
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThirdPartyReportAddresses
**Note**: You don't need to use this parameter. You specify the email address of the user submissions mailbox using the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet. To reduce confusion, you can set this parameter to the same value.

Use the ThirdPartyReportAddresses parameter to specify the email address of the user submissions mailbox when you're using a third-party product for user submissions instead of the Microsoft integrated reporting experience.

For more information about disabling the Microsoft integrated reporting experience in favor of a third-party product, see the EnableThirdPartyAddress parameter.

```yaml
Type: MultiValuedProperty
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserSubmissionOptions
The UserSubmissionOptions parameter specifies the reporting options that are available to users. Valid values are:

- 0: No reporting options are available. This value is available only when the DisableUserSubmissionOptions parameter value is $false.
- 1: **Ask before reporting the message** is the only available option.
- 2: **Always report the message** is the only available option.
- 3: **Ask before reporting the message** and **Always report the message** are available. This value is available only when the DisableUserSubmissionOptions parameter value is $false.
- 4: **Never report the message** is the only available option.
- 5: **Ask before reporting the message** and **Never report the message** are available. This value is available only when the DisableUserSubmissionOptions parameter value is $false.
- 6: **Always report the message** and **Never report the message** are available. This value is available only when the DisableUserSubmissionOptions parameter value is $false.
- 7: **Ask before reporting the message**, **Always report the message**, and **Never report the message** are available. This value is available only when the DisableUserSubmissionOptions parameter value is $false.

This parameter is meaningful only when the Microsoft integrated reporting experience is enabled as described in the EnableReportToMicrosoft parameter.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserSubmissionOptionsMessage
This parameter is reserved for internal Microsoft use.

```yaml
Type: String
Parameter Sets: (All)
Aliases:
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
The WhatIf switch simulates the actions of the command. You can use this switch to view the changes that would occur without actually applying those changes. You don't need to specify a value with this switch.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi
Applicable: Exchange Online

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS