# Editing or deleting a 10DLC campaign<a name="sns-edit-campaign-10dlc"></a>

You can edit the HELP response, STOP response, and the sample messages for a 10DLC campaign using the Amazon Pinpoint console\. You can also delete 10DLC campaigns using the console\.

## Editing a 10DLC campaign<a name="sns-editing-10dlc-campaign"></a>

After your campaign is approved, you can modify the HELP, STOP, and sample messages\. You can also add additional sample messages\. Changes to these fields don't require re\-approval from the Campaign Registry or from carriers\. You can't modify any other fields after the 10DLC campaign is approved\.

You can have a maximum of five sample messages\. You can't reduce the number of sample messages that you originally registered\. For example, if you registered your campaign with three sample SMS messages, you can't reduce the number of sample SMS messages to less than three\. 

**Note**  
If you want to modify any fields other than the HELP, STOP, and sample messages, you must first delete the 10DLC campaign and then recreate the campaign to include the updated information\.  
 

**To edit a 10DLC campaign**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, choose the 10DLC campaign that you want to edit\.

1. In the** Campaign messages** section of the campaign details page, choose **Edit\.**

1. Update any of the following fields:
   + **Help message**
   + **Stop message**
   + **Sample SMS message**

   You can't delete a previously added sample message, or delete the contents of a sample message so that the field is empty\. If you delete the contents of a message without replacing that content, the original message will be used on updating\.

1. Choose **Update**\. A confirmation banner appears letting you know the campaign messages were updated\.

## Deleting a 10DLC campaign<a name="sns-deleting-10dlc-campaign"></a>

You can delete a 10DLC campaign using the Amazon Pinpoint console\. Before you delete a 10DLC campaign, you must first remove all of the phone numbers associated with that campaign\. 

**Important**  
When you remove a 10DLC number from a campaign, you no longer have access to that number\. Additionally, deleted 10DLC campaigns can't be restored\.  
 

**To delete a 10DLC campaign**

1. Sign in to the AWS Management Console and open the Amazon Pinpoint console at [https://console\.aws\.amazon\.com/pinpoint/](https://console.aws.amazon.com/pinpoint/)\.

1. In the navigation pane, under **SMS and voice**, choose **Phone numbers**\.

1. On the **10DLC campaigns** tab, choose the 10DLC campaign that you want to edit\.

1. In the **Phone numbers** section, note the phone numbers associated with the campaign\.

1. On the **Phone numbers** tab, choose the 10DLC number you want to remove, and then choose **Remove phone number**\.
**Note**  
This step is only required if you have multiple 10DLC phone numbers associated with the campaign\. If you have only a single phone number associated with the 10DLC campaign that number will appear on the **10DLC campaigns** tab\. Note the number displayed on the tab\.

1. Enter **delete** into the confirmation box, and then choose **Confirm**\. A success message appears at the top of the SMS and voice page\.

1. Repeat the previous two steps for each 10DLC number associated with the campaign\.

1. After removing any numbers associated with the 10DLC campaign, choose the **10DLC campaigns** tab\.

1. Choose the 10DLC campaign you want to delete\.

1. In the upper right\-hand corner of the **10DLC campaign details** page, choose **Delete**\.

1. Enter **delete** into the confirmation box, and then choose **Confirm**\. A success message appears at the top of the SMS and voice page\.