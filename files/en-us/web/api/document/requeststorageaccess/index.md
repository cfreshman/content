---
title: Document.requestStorageAccess()
slug: Web/API/Document/requestStorageAccess
page-type: web-api-instance-method
tags:
  - API
  - DOM
  - Document
  - Method
  - Reference
  - Storage Access API
  - requestStorageAccess
browser-compat: api.Document.requestStorageAccess
---

{{APIRef("Storage Access API")}}

The **`requestStorageAccess()`** method of the {{domxref("Document")}} interface returns a {{jsxref("Promise")}} that resolves if the access to first-party storage was granted, and rejects if access was denied.

This is part of the [Storage Access API](/en-US/docs/Web/API/Storage_Access_API).

## Conditions for granting storage access

Storage access is granted based on a series of checks described here:

1. If the browser is not processing a user gesture, reject (see {{Glossary("Transient activation")}}).
2. If the document already has been granted access, resolve.
3. If the document has a null origin, reject.
4. If the document's frame is the main frame, resolve.
5. If the sub frame's origin is equal to the main frame's, resolve.
6. If the sub frame is not sandboxed, skip to step 7.
7. If the sub frame doesn't have the token `allow-storage-access-by-user-activation`, reject.
8. If the sub frame's parent frame is not the top frame, reject.
9. Check any additional rules that the browser has. Examples: allow lists, block lists, on-device classification, user settings, anti-[clickjacking](/en-US/docs/Glossary/Clickjacking) heuristics, or prompting the user for explicit permission.
   Reject if some rule is not fulfilled.
10. Grant the document access to cookies and other site storage and store that fact for the purposes of future calls to {{domxref("Document.hasStorageAccess()")}} and `requestStorageAccess()`.

Assuming all of the requirements above are satisfied, Firefox will automatically grant storage access to the requesting origin on up to a threshold number of first-party sites in the current session for the duration of user's session, up to a maximum of 24 hours.
After the requesting origin has exceeded the maximum allowable number of storage access grants, any future call to `requestStorageAccess()` during the same browsing session will prompt the user.

The maximum number of concurrent storage access grants an origin can obtain is a positive integer currently defined as one percent of the number of top-level sites visited in the current session or 5, whichever is higher.
The threshold is enforced on the level of site, so for example two storage access grants for `foo.example.com` and `bar.example.com` will only count as a single exception against the limit.

At the time of a `requestStorageAccess()` call, if the requesting origin has
storage access to fewer sites than the maximum and has been interacted with as a first party in the last 30 days:

- The user is not prompted.
- The origin is given an ephemeral storage access grant for the current top-level site.
- The number of sites the requesting origin has storage access to is incremented by one.

  - Note that this number is also incremented when automatic access grants are given through [Firefox compatibility heuristics](/en-US/docs/Web/Privacy/Storage_Access_Policy#automatic_storage_access_upon_interaction).

- The ephemeral storage access grant is:

  - Invalidated at the end of the browser session.
  - Not persisted to disk (e.g. will not persist if the browser crashes).
  - Reset after 24 hours in the case of a long-running browser session.

When equal or more sites than the maximum or has not been interacted with as a first party in the last 30 days:

- The user is prompted
- If the user clicks "Allow" or "Allow on any site" the request is resolved.
- If the user clicks "Don't Allow", the storage access request is rejected and the requesting origin can re-request once it receives another user interaction.
- If the user allows storage the requesting origin is given a persistent storage access grant on the current top-level site.
- The number of sites the requesting origin has storage access to is incremented by one.
- The persistent storage access permission is:

  - Persisted to disk and will remain valid in future browser sessions.
  - Reset after 30 days.

When an ephemeral or persistent storage access grant expires, the number of sites the requesting origin has storage access to is decremented by one.

> **Note:** If the requesting origin is not [classified as a tracking origin](/en-US/docs/Web/Privacy/Storage_Access_Policy#tracking_protection_explained), the access request is automatically given an ephemeral storage access grant, which will go away when the page is reloaded.
> The user is never shown a prompt in this case, and calling `requestStorageAccess()` won't have any side effects besides changing the value returned by {{domxref("Document.hasStorageAccess()")}}.

## Debugging

The storage access grant threshold may make it more difficult to test your website under the condition where Firefox prompts the user for access.
To make testing easier, we have added two preferences in `about:config` that control prompting upon `requestStorageAccess()` calls:

- `dom.storage_access.auto_grants` can be set to `false` to disable the automatic granting of ephemeral storage access grants.
  All calls to `requestStorageAccess()` by origins classified as trackers will trigger a prompt.
- `dom.storage_access.max_concurrent_auto_grants` controls the threshold number of storage access grants at which users will begin to receive prompts.
  For example, if you want to configure Firefox to automatically grant access on the first site where `requestStorageAccess()` is called and then prompt afterwards, you should adjust the value of the `dom.storage_access.max_concurrent_auto_grants` preference to 1.

## Syntax

```js-nolint
requestStorageAccess()
```

### Parameters

None.

### Return value

A {{jsxref("Promise")}} that fulfills with `undefined` if the access to first-party storage was granted, and rejects if access was denied.

When the promise gets resolved, the resolve handler will run as if a user gesture is being processed, whether the promise was fulfilled or rejected:

- In the former case, code can then start to call APIs that require user activation and things can move forward.
- In the latter case, code can run to inform the user of why the request failed and what they can do to continue (for example asking them to log in, if that is a requirement).

## Examples

```js
document.requestStorageAccess().then(
  () => { console.log('access granted') },
  () => { console.log('access denied') }
);
```

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Using the Storage Access API](/en-US/docs/Web/API/Storage_Access_API/Using)
- [Introducing Storage Access API](https://webkit.org/blog/8124/introducing-storage-access-api/) (WebKit blog)
