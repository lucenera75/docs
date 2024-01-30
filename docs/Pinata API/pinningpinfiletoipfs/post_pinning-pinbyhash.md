---
title: "Pin By CID"
slug: "post_pinning-pinbyhash"
excerpt: "Upload a file already on the IPFS network to Pinata"
hidden: false
metadata: 
  image: []
  robots: "index"
createdAt: "Fri Jul 07 2023 19:48:25 GMT+0000 (Coordinated Universal Time)"
updatedAt: "Thu Sep 21 2023 00:49:07 GMT+0000 (Coordinated Universal Time)"
---
There may be times where a piece of content is not available on your local machine or server but is already on the IPFS network. In those times, the Pin By CID API endpoint is helpful. A CID (or content identifier) is a hash generated by the IPFS protocol and representative of a piece of content. By taking that CID (or hash) and using this API, you can import the content into your own Pinata account and pin it on Pinata's storage nodes. 

## Pinning a File By CID

Each file pinned can optionally include additional information beyond just the file. Both `pinataOptions` and `pinataMetadata` can be included in the request. Their formats [are documented here](ref:pinningpinfiletoipfs).

This endpoint allows for an additional property in the `pinataOptions` object to help our IPFS nodes find the content you would like pinned. 

**`hostNodes` - multiaddresses of nodes your content is already stored on.** 

You can pass in the "multiaddresses" up to five host nodes that your content already resides on.

To find the multiaddresses of your own nodes, simply run the following on your node's command line:

`ipfs id`

In the response, you'll want to focus on the "Addresses" array that's returned. Here you'll find the multiaddresses of your node. These multiaddresses are what other IPFS nodes use to connect to your node.

In the "Addresses" array, take note of the multiaddress that contains your external IP address. Not the local ipv4 "127.0.0.1" address or the local ipv6 "::1" address.

Here's an example of a full external ipv4 multiaddress (your IP address and node ID will differ):

`/ip4/123.456.78.90/tcp/4001/ipfs/QmAbCdEfGhIjKlMnOpQrStUvWxYzAbCdEfGhIjKlMnOpQr`

Once you've grabbed the full multiaddress for every node that already has your content pinned, simply add the "host\_nodes" property to your pinataOptions object like so:

```
{
    hashToPin: (ExampleHash),
    pinataOptions: {
        hostNodes: [
            /ip4/hostNode1ExternalIP/tcp/4001/ipfs/hostNode1PeerId,
            /ip4/hostNode2ExternalIP/tcp/4001/ipfs/hostNode2PeerId
            .
            .
            .
        ]
    }
}
```