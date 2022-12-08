---
external help file: Microsoft.Exchange.TransportMailflow-Help.xml
online version: https://learn.microsoft.com/powershell/module/exchange/new-reportsubmissionpolicy
applicable: Exchange Online
title: New-ReportSubmissionPolicy
schema: 2.0.0
author: chrisda
ms.author: chrisda
ms.reviewer:
---

# New-ReportSubmissionPolicy

## SYNOPSIS
This cmdlet is available only in the cloud-based service.

Use the New-ReportSubmissionPolicy cmdlet to create the report submission policy in your cloud-based organization. The report submission policy controls most of the user submission settings in the organization.

**Note**: If the policy already exists (the Get-ReportSubmissionPolicy cmdlet returns output), you can't use this cmdlet. To delete the existing policy and start over with the default settings, use the Remove-ReportSubmissionPolicy cmdlet first.

For information about the parameter sets in the Syntax section below, see [Exchange cmdlet syntax](https://learn.microsoft.com/powershell/exchange/exchange-cmdlet-syntax).

## SYNTAX

```
New-ReportSubmissionPolicy
 [-DisableQuarantineReportingOption <Boolean>]
 [-DisableUserSubmissionOptions <Boolean>]
 [-EnableCustomizedMsg <Boolean>]
 [-EnableCustomNotificationSender <Boolean>]
 [-EnableOrganizationBranding <Boolean>]
 [-EnableReportToMicrosoft <Boolean>]
 [-EnableThirdPartyAddress <Boolean>]
 [-EnableUserEmailNotification <Boolean>]
 [-JunkReviewResultMessage <String>]
 [-NotificationFooterMessage <String>]
 [-NotificationSenderAddress <MultiValuedProperty>]
 [-NotJunkReviewResultMessage <String>]
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
 [<CommonParameters>]
```

## DESCRIPTION
The report submission policy controls most of the settings for user submissions in the Microsoft 365 Defender portal at <https://security.microsoft.com/userSubmissionsReportMessage>.

The report submission rule (\*-ReportSubmissionRule cmdlets) controls the email address of the user submissions mailbox where user reported messages are sent.

When you set the email address of the user submissions mailbox in the Microsoft 365 Defender portal at <https://security.microsoft.com/userSubmissionsReportMessage>, the same email address is also set in the following parameters in the \*-ReportSubmissionPolicy cmdlets:

- Microsoft integrated reporting: The ReportJunkAddresses, ReportNotJunkAddresses, and ReportPhishAddresses parameters.
- Third-party tools: The ThirdPartyReportAddresses parameter.

You need to be assigned permissions before you can run this cmdlet. Although this topic lists all parameters for the cmdlet, you may not have access to some parameters if they're not included in the permissions assigned to you. To find the permissions required to run any cmdlet or parameter in your organization, see [Find the permissions required to run any Exchange cmdlet](https://learn.microsoft.com/powershell/exchange/find-exchange-cmdlet-permissions).

## EXAMPLES

### Example 1
```powershell
New-ReportSubmissionPolicy 
```

This example creates the one and only report submission policy named DefaultReportSubmissionPolicy with the default values (users can report messages only to Microsoft, and the user submissions mailbox isn't used).

### Example 2
```powershell
$usersub = "reportedmessages@contoso.com"

New-ReportSubmissionPolicy -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub -DisableUserSubmissionOptions $false -EnableCustomizedMsg $false -OnlyShowPhishingDisclaimer $true -EnableCustomNotificationSender $false -EnableOrganizationBranding $false -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example creates the report submission policy with the Microsoft integrated reporting experience enabled, user reporting to Microsoft enabled, and reported messages sent to the specified user submissions mailbox.

**Notes**:

- The default value of the EnableReportToMicrosoft parameter is $true and the default value of the EnableThirdPartyAddress parameter is $false, so you don't need to use them.
- To create the policy, you need to specify the same email address in the ReportJunkAddresses, ReportNotJunkAddresses, and ReportPhisAddresses parameters, and also in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.
- The remaining parameters in the New-ReportSubmissionPolicy command are required to create the policy. The default values of those parameters are used.
- Like the report submission policy, you can create the report submission rule only if it doesn't already exist. If the rule already exists, you can use Set-ReportSubmissionRule to change the email address of the user submissions mailbox, or Remove-ReportSubmissionRule to delete it and recreate it.

### Example 3
```powershell
$usersub = "userreportedmessages@fabrikam.com"

New-ReportSubmissionPolicy -EnableReportToMicrosoft $false -ReportJunkToCustomizedAddress $true -ReportJunkAddresses $usersub -ReportNotJunkToCustomizedAddress $true -ReportNotJunkAddresses $usersub -ReportPhishToCustomizedAddress $true -ReportPhishAddresses $usersub -DisableUserSubmissionOptions $false -EnableCustomizedMsg $false -OnlyShowPhishingDisclaimer $true -EnableCustomNotificationSender $false -EnableOrganizationBranding $false -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example creates the report submission policy with the Microsoft integrated reporting experience enabled, user reporting to Microsoft disabled, and reported messages sent to the specified user submissions mailbox only.

### Example 4
```powershell
$usersub = "thirdpartyreporting@wingtiptoys.com"

New-ReportSubmissionPolicy -EnableReportToMicrosoft $false -EnableThirdPartyAddress $true -ThirdPartyReportAddresses $usersub -DisableQuarantineReportingOption $false

New-ReportSubmissionRule -Name DefaultReportSubmissionRule -ReportSubmissionPolicy DefaultReportSubmissionPolicy -SentTo $usersub
```

This example creates the report submission policy with the Microsoft integrated reporting experience disabled, and user reporting via third-party tools to the specified user submissions mailbox.

## PARAMETERS

### -DisableQuarantineReportingOption
The DisableQuarantineReportingOption parameter allows or prevents users from reporting messages in quarantine. Valid values are:

- $true: Users can't report quarantined messages from quarantine.
- $false: Users can report quarantined messages from quarantine. This is the default value.

This parameter is required to create the report submission policy if you're sending reported messages to the user submissions mailbox (Microsoft integrated reporting experience or third-party tools).

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

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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

The value $true for this parameter enables the Microsoft integrated reporting experience. The following results are possible:

- **Users can report messages to Microsoft only (the user submission mailbox isn't used)**: The ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameter values are $false. This is the default result.
- **Users can report messages to Microsoft and reported messages are sent to the user submissions mailbox**: The ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameter values are $true. To create the policy, use the same email address in the ReportJunkAddresses, ReportNotJunkAddresses, and ReportPhisAddresses parameters, and also in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

The value $false for this parameter has the following possible results:

- **The Microsoft integrated reporting experience is enabled, but reported messages are sent only to the user submissions mailbox**: The ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameter values are $true. To create the policy, use the same email address in the ReportJunkAddresses, ReportNotJunkAddresses, and ReportPhisAddresses parameters, and also in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.
- **The Microsoft integrated reporting experience is disabled. Third-party tools send reported messages to the user submissions mailbox**: The EnableThirdPartyAddress parameter value is $true. To create the policy, use the same email address in the ThirdPartyReportAddresses parameter and also in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

This parameter is required to create the report submission policy only if you set the value to $false (the default value is $true).

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
The EnableThirdPartyAddress parameter specifies whether you're using a third-party product instead of the Microsoft integrated reporting experience to send messages to the user submissions mailbox. Valid values are:

- $true: The Microsoft integrated reporting experience is disabled. Third-party tools send reported messages to the user submissions mailbox. You also need to set the EnableReportToMicrosoft parameter value to $false. To create the policy, use the same email address in the ThirdPartyReportAddresses parameter and also in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.
- $false: The Microsoft integrated reporting experience is enabled. This is the default value. Use the EnableReportToMicrosoft parameter to control whether user reported messages are sent to the user submissions mailbox, and whether users can report messages to Microsoft.

This parameter is required to create the report submission policy only if you set the value to $true (the default value is $false).

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

### -OnlyShowPhishingDisclaimer
The OnlyShowPhishingDisclaimer parameter specifies whether to show users the before reporting and after reporting messages only for messages that were reported as phishing. Valid values are:

- $true: Show the before reporting and after reporting messages only when users report messages as phishing. This is the default value.
- $false: Show the before reporting and after reporting messages when users report messages as junk, not junk, and phishing.

You specify the title and message body for the before reporting and after reporting messages using the following parameters:

- PreSubmitMessageTitle
- PreSubmitMessage
- PostSubmitMessageTitle
- PostSubmitMessage

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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
The ReportJunkAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

This parameter is required to create the report submission policy if the ReportJunkToCustomizedAddress parameter value is $true.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters.

You also need to specify the same email address in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

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

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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
The ReportNotJunkAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

This parameter is required to create the report submission policy if the ReportNotJunkToCustomizedAddress parameter value is $true.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters.

You also need to specify the same email address in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

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

You can't use this parameter by itself. You need to specify the same value for the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters.

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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
The ReportPhishAddresses parameter specifies the email address of the custom Exchange Online mailbox to receive user reported messages in the Microsoft integrated reporting experience.

This parameter is required to create the report submission policy if the ReportPhishToCustomizedAddress parameter value is $true.

You can't use this parameter by itself. You need to specify the same email address for the ReportJunkAddresses, ReportNotJunkAddresses and ReportPhishAddresses parameters.

You also need to specify the same email address in the SentTo parameter on the New-ReportSubmissionRule or Set-ReportSubmissionRule cmdlet.

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

You can't use this parameter by itself. You need to specify the same value for the ReportJunkToCustomizedAddress, ReportNotJunkToCustomizedAddress, and ReportPhishToCustomizedAddress parameters.

This parameter is required to create the report submission policy if you're using the Microsoft integrated reporting experience (see the EnableReportToMicrosoft parameter) and sending reported messages to the user submissions mailbox (exclusively or in addition to reporting to Microsoft).

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
Use the ThirdPartyReportAddresses parameter to specify the email address of the user submissions mailbox when you're using a third-party product for user submissions instead of the Microsoft integrated reporting experience.

This parameter is required to create the report submission policy if you've disabled the Microsoft integrated reporting experience (`-EnableReportToMicrosoft $false`) and you're using the user submissions mailbox with third-party tools (`-EnableThirdPartyAddress $true`).

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

### CommonParameters
This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable, -InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose, -WarningAction, and -WarningVariable. For more information, see [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## INPUTS

## OUTPUTS

## NOTES

## RELATED LINKS