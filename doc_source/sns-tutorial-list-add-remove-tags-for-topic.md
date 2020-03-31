# Tutorial: Listing, adding, and removing tags for an Amazon SNS topic<a name="sns-tutorial-list-add-remove-tags-for-topic"></a>

You can track your Amazon SNS resources \(for example, for cost allocation\) by adding, removing, and listing metadata tags for Amazon SNS topics\. The following tutorial shows how to add, update, and remove tags for a topic using the AWS Management Console and the AWS SDK for Java\. For more information, see [Amazon SNS tags](sns-tags.md)\.

**Topics**
+ [AWS Management Console](#add-update-remove-tags-for-topic-aws-console)
+ [AWS SDK for Java](#add-update-remove-tags-for-topic-aws-java)

**Note**  
Currently, tag\-based access control isn't available\.

## To list, add, and remove, metadata tags for an Amazon SNS topic using the AWS Management Console<a name="add-update-remove-tags-for-topic-aws-console"></a>

1. Sign in to the [Amazon SNS console](https://console.aws.amazon.com/sns/home)\.

1. On the navigation panel, choose **Topics**\.

1. On the **Topics** page, choose a topic and choose **Edit**\.

1. Expand the **Tags** section\.

   The tags added to the topic are listed\.

1. Modify topic tags:
   + To add a tag, choose **Add tag** and enter a **Key** and **Value** \(optional\),
   + To remove a tag, choose **Remove tag** next to a key\-value pair\.

1. Choose **Save changes**

## To list, add, and remove metadata tags for an Amazon SNS topic using the AWS SDK for Java\.<a name="add-update-remove-tags-for-topic-aws-java"></a>

1. Specify your AWS credentials\. For more information, see [Set up AWS Credentials and Region for Development](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/setup-credentials.html) in the *AWS SDK for Java 2\.x Developer Guide*\.

1. Write your code\. For more information, see [Using the SDK for Java 2\.x](https://docs.aws.amazon.com/sdk-for-java/v2/developer-guide/basics.html)\.

1. To list the tags added to a topic, add the following code:

   ```
   final ListTagsForResourceRequest listTagsForResourceRequest = new ListTagsForResourceRequest();
   listTagsForResourceRequest.setResourceArn(topicArn);
   final ListTagsForResourceResult listTagsForResourceResult = snsClient.listTagsForResource(listTagsForResourceRequest);
   System.out.println(String.format("ListTagsForResource: \tTags for topic %s are %s.\n",
   	topicArn, listTagsForResourceResult.getTags()));
   ```

1. To add tags \(or update the value of tags\), add the following code:

   ```
   final Tag tagTeam = new Tag();
   tagTeam.setKey("Team");
   tagTeam.setValue("Development");
   final Tag tagEnvironment = new Tag();
   tagEnvironment.setKey("Environment");
   tagEnvironment.setValue("Gamma");
        
   final List<Tag> tagList = new ArrayList<>();
   tagList.add(tagTeam);
   tagList.add(tagEnvironment);
        
   final TagResourceRequest tagResourceRequest = new TagResourceRequest();
   tagResourceRequest.setResourceArn(topicArn);
   tagResourceRequest.setTags(tagList);
   final TagResourceResult tagResourceResult = snsClient.tagResource(tagResourceRequest);
   ```

1. To remove a tag from the topic using the tag's key, add the following code:

   ```
   final UntagResourceRequest untagResourceRequest = new UntagResourceRequest();
   untagResourceRequest.setResourceArn(topicArn);
   final List<String> tagKeyList = new ArrayList<>();
   tagKeyList.add("Team");
   untagResourceRequest.setTagKeys(tagKeyList);
   final UntagResourceResult untagResourceResult = snsClient.untagResource(untagResourceRequest);
   ```

1. Compile and run your code\.

   The existing tags are listed, two are added, and one is removed from the topic\.