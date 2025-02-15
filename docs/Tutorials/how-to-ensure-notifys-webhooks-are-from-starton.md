---
title: How to ensure Monitor's webhooks are from Starton
displayedSidebar: tutorialSidebar
---

# How to ensure Monitor's webhooks are from Starton

Using Monitor might be very useful to receive webhooks whenever specific events happen on-chain.

However, we need to be careful when receiving webhooks: someone could try to send a webhook to your endpoint, pretending to be Starton.

To protect our users against this kind of attacks we provide a signature within the request’s headers.

You can then compare this signature to the one you get when signing the payload yourself.

Let’s see in details the process of verifying the signature with some JS example.

## Get the signature in the header

The signature is an hexadecimal string that looks like `ac7c2691a5f632ad4fc2f8b225fd6f25e3526f12293c32efbcb5999527746037.`

You can find it in the headers with the key starton-signature.

```jsx
const reqSignature = req.headers["starton-signature"]
```

## Find your signing key on Starton

To be able to sign the message you'll need to use the same signing key that was used to get the starton-signature.

This webhook signing key is available in the [Developer](/Developer/Discovering-coding-interface.md) section of our dashboard.

It should look like: `whs_lDtcN3gDEaY_Yg3aony8077Lr-9wa5JyxAitUvfg886y9JDs4mVoY9uD`.

<div class="row-is-multiline">

<div class="col col--2" class="cards">
	<a class="button-card button-card--vertical" href="https://app.starton.com/projects">
		<h3>Get your signing key from the Dashboard</h3>
		<div class="button-card__inner">
			<p color="white">Go to <b>Starton Dashboard</b> and get your signing key in the developer section.</p>
		</div>
	</a>
</div>

</div>

## Sign the payload using the signing key

To properly compute the signature you will have to compute an HMAC with the SHA256 hash function.

You must provide the signing key as the secret key and the full payload you received as the message to sign.

If you code in JS / TS you can use the node-forge module to compute the HMAC.

```jsx
import { hmac } from "node-forge";
​
const secretKey = "whs_lDtcN3gDEaY_Yg3aony8077Lr-9wa5JyxAitUvfg886y9JDs4mVoY9uD";
const payload = { ... }
​
const stream = hmac.create();
stream.start("sha256", secretKey);
stream.update(Buffer.from(JSON.stringify(payload)));
​
const localSignature = stream.digest().toHex();
console.log(localSignature);
```

## Check that your signature is the same as the one provided in the header

Great! We can now check if the signature sent in the webhook matches the one we computed locally.

```jsx
if (!reqSignature || localSignature !== reqSignature) {
	throw "Signature doesn't match"
}
```

And that's it! You should now be able to check and guarantee that the webhooks are coming from Starton and not from an evil mind!
