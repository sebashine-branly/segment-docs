---
title: Heap Destination
rewrite: true
id: 54521fd725e721e32a72eebd
---
[Heap](https://heapanalytics.com/?utm_source=segmentio&utm_medium=docs&utm_campaign=partners) automatically captures every user interaction with no extra code. This includes clicks, taps, gestures, form submissions, page views, and more. The Heap Destination is open-source. You can browse the code [on GitHub](https://github.com/segment-integrations/analytics.js-integration-heap).

## Getting Started



1. From the Segment web app, click **Catalog**.
2. Search for "Heap" in the Catalog, select it, and choose which of your sources to connect the destination to.
3. In the destination settings, enter your Heap "App ID" into the connection settings.
4. Remove Heap's snippet from your page if you're using Segment's client-side Analytics.js library to load Heap. With the Analytics.js library, Segment asynchronously loads Heap's JavaScript library onto your page. All native functionality of Heap, including auto-capturing of all events are available to you.  

## Identify

If you're not familiar with the Segment Specs, take a look to understand what the [Identify method](/docs/connections/spec/identify/) does. An example call would look like:

```javascript
analytics.identify('userId123');
```

When you call `identify`, Segment calls [Heap's identify method](https://developers.heap.io/docs/using-identify){:target="_blank"} with the `userId` and `traits` you provide.

If one of your `traits` is of the date property type, we will convert it into an ISO string.

## Track

If you're not familiar with the Segment Specs, take a look to understand what the [Track method](/docs/connections/spec/track/) does. An example call would look like:

```javascript
analytics.track('Clicked Button');
```

When you call `track` from `analytics.js`, Segment calls Heap's [track function](https://developers.heap.io/reference/track){:target="_blank"} with the same parameters. Calling `track` from one of Segment's mobile SDKs or server-side sources records a [Heap Custom Event](https://docs.heapanalytics.com/reference#track-1){:target="_blank"} with the same event name and properties.

If one of your `properties` is of the date property type, we will convert it into an ISO string.

## Troubleshooting

### Anonymous traffic not showing up from server-side source
Ensure that you are specifying a `user_id` in your event calls. The Heap API will reject any server-side events without a `user_id`.

### Nested Objects and Arrays
Heap does not accept nested properties, so we will flatten and stringify them before sending to Heap. For example:

 ```javascript
 analytics.track('Signed Up', {
   foo: {
     bar: {
       cheese: 'american',
       prop: [1, 2, 3],
       products: [{"A": "Jello"}, {"B": "Peanut"}]
     }
   }
 });
 ```

The properties would be sent as:

```javascript
foo.bar.cheese: 'american'
foo.bar.prop: '[1,2,3]'
foo.bar.products: "[{'A': 'Jello'},{'B': 'Peanut'}]"
```
