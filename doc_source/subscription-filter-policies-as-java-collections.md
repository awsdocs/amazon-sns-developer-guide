# Subscription filter policies as Java collections<a name="subscription-filter-policies-as-java-collections"></a>

To provide a subscription filter policy to the Amazon SNS client using the AWS SDK for Java, you must pass it as a string using the following process:

1. To define your policy as a collection, create a map that associates each attribute name with a list of values\.

1. To assign the policy to a subscription, produce a string version of the policy from the contents of the map\.

1. Pass the string to the Amazon SNS client\.

## Working Java example<a name="subscription-filter-policies-working-java-example"></a>

The following example Java code demonstrates the process of providing the subscription filter policy to the Amazon SNS client\.

```
/*
 * Copyright 2010-2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.
 *
 * Licensed under the Apache License, Version 2.0 (the "License").
 * You may not use this file except in compliance with the License.
 * A copy of the License is located at
 *
 *  https://aws.amazon.com/apache2.0
 * 
 * or in the "license" file accompanying this file. This file is distributed
 * on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either
 * express or implied. See the License for the specific language governing
 * permissions and limitations under the License.
 *
 */

import com.amazonaws.services.sns.AmazonSNS;
import com.amazonaws.services.sns.model.SetSubscriptionAttributesRequest;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

// The class stores the filterPolicy field as a map.
public class SNSMessageFilterPolicy {

    private final Map<String, Attribute> filterPolicy = new HashMap<>();

    // You can use the addAttribute(), addAttributePrefix(), and addAttributeAnythingBut()
    // methods to add attributes to your policy. These methods accept the attribute name as a string, 
    // and they are specialized to accept different value types. You can pass values as strings, 
    // lists of strings, numbers, or number ranges and you can also add the attributes anything-but 
    // and prefix.
    public void addAttribute(final String attributeName, final String attributeValue) {
        filterPolicy.put(attributeName, new Attribute<>(AttributeType.String, attributeValue));
    }

    public void addAttribute(final String attributeName, final ArrayList<String> attributeValues) {
        final ArrayList<Attribute> attributes = new ArrayList<>();
        for (final String s : attributeValues) {
            attributes.add(new Attribute<>(AttributeType.String, s));
        }
        filterPolicy.put(attributeName, new Attribute<>(AttributeType.List, attributes));
    }

    public void addAttributePrefix(final String attributeName, final String prefix) {
        filterPolicy.put(attributeName, new Attribute<>(AttributeType.Prefix, prefix));
    }

    public void addAttributeAnythingBut(final String attributeName, final String value) {
        filterPolicy.put(attributeName, new Attribute<>(AttributeType.AnythingBut, value));
    }

    public <T extends Number> void addAttribute(final String attributeName, final String op, final T value) {
        filterPolicy.put(attributeName, new Attribute<>(AttributeType.Numeric, new NumericValue<>(op, value)));
    }

    public <T extends Number> void addAttributeRange(
            final String attributeName,
            final String lowerOp,
            final T lower,
            final String upperOp,
            final T upper) {
        filterPolicy.put(attributeName,
                new Attribute<>(AttributeType.Numeric, new NumericValue<>(lowerOp, lower, upperOp, upper)));
    }

    // To apply your policy to a subscription, use the apply() method, providing the client and the 
    // subscription ARN. This method produces a policy string from the contents of the filterPolicy map, 
    // and then applies the policy to the specified subscription.
    public void apply(final AmazonSNS snsClient, final String subscriptionArn) {
        final SetSubscriptionAttributesRequest request =
                new SetSubscriptionAttributesRequest(subscriptionArn,
                        "FilterPolicy", formatFilterPolicy());
        snsClient.setSubscriptionAttributes(request);
    }

    private String formatFilterPolicy() {
        return filterPolicy.entrySet()
                .stream()
                .map(entry -> "\"" + entry.getKey() + "\": [" + entry.getValue() + "]")
                .collect(Collectors.joining(", ", "{", "}"));
    }

    private enum AttributeType {
        String, Numeric, Prefix, List, AnythingBut
    }

    private class Attribute<T> {
        final T value;
        final AttributeType type;

        Attribute(final AttributeType type, final T value) {
            this.value = value;
            this.type = type;
        }

        public String toString() {
            switch (type) {
                case Prefix:
                    return String.format("{\"prefix\":\"%s\"}", value.toString());
                case Numeric:
                    return String.format("{\"numeric\":%s}", value.toString());
                case List:
                    final List list = (List)value;
                    final ArrayList<T> values = new ArrayList<T>(list);
                    return values
                            .stream()
                            .map(Object::toString)
                            .collect(Collectors.joining(","));
                case AnythingBut:
                    return String.format("{\"anything-but\":\"%s\"}", value);
                default:
                    return String.format("\"%s\"", value);
            }
        }
    }

    private class NumericValue<T extends Number> {
        private final T lower;
        private final T upper;
        private final String lowerOp;
        private final String upperOp;

        NumericValue(final String op, final T value) {
            lower = value;
            lowerOp = op;
            upper = null;
            upperOp = null;
        }

        NumericValue(final String lowerOp, final T lower, final String upperOp, final T upper) {
            this.lower = lower;
            this.lowerOp = lowerOp;
            this.upper = upper;
            this.upperOp = upperOp;
        }

        public String toString() {
            final StringBuilder s = new StringBuilder("[")
                    .append('\"')
                    .append(lowerOp)
                    .append("\",")
                    .append(lower);
            if (upper != null) {
                s.append(",\"")
                        .append(upperOp).
                        append("\",").
                        append(upper);
            }
            s.append("]");
            return s.toString();
        }
    }
}
```

The following Java code excerpt shows how to initialize and use the example `SNSMessageFilterPolicy` class:

```
// Initialize the example filter policy class.
final SNSMessageFilterPolicy fp = new SNSMessageFilterPolicy();

// Add a filter policy attribute with a single value.
fp.addAttribute("store", "example_corp");
fp.addAttribute("event", "order_placed");

// Add a prefix attribute.
filterPolicy.addAttributePrefix("customer_interests", "bas");

// Add an anything-but attribute.
filterPolicy.addAttributeAnythingBut("customer_interests", "baseball");

// Add a filter policy attribute with a list of values.
final ArrayList<String> attributeValues = new ArrayList<>();
attributeValues.add("rugby");
attributeValues.add("soccer");
attributeValues.add("hockey");
fp.addAttribute("customer_interests", attributeValues);

// Add a numeric attribute.
filterPolicy.addAttribute("price_usd", "=", 0);

// Add a numeric attribute with a range.
filterPolicy.addAttributeRange("price_usd", ">", 0, "<=", 100);

// Apply the filter policy attributes to an Amazon SNS subscription.
fp.apply(snsClient, subscriptionArn);
```