---
id: KB-D3328AFF
subject: "Wishlist sharing deprecation shape: no plural sharingSettings, singular sharingSetting not deprecated, sharedWithId deprecated"
plane: experiential
question: Is WishlistType.sharingSetting or SharingSettingType.sharedWithId deprecated in the multi-target sharing build, and is there a plural sharingSettings field?
questions:
  - text: Which wishlist sharing fields are being phased out in the new multi-recipient sharing build?
  - text: WishlistType.sharingSetting SharingSettingType.sharedWithId isDeprecated introspection
  - text: Is there a plural sharingSettings field on WishlistType?
  - text: What replaces SharingSettingType.sharedWithId and what fields does SharingTargetType have?
  - text: Do InputChangeWishlistType and InputCreateWishlistType still accept sharedWithId?
concepts:
  - id: list-sharing
  - id: wishlist
  - id: graphql-api
status: active
appliesTo:
  - axis: surface
    value: xapi
anchors:
  - coordinate: WishlistType.sharingSetting
  - coordinate: SharingSettingType.sharedWithId
evidence:
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:36:49.626Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
  - method: observation
    deployment: vcptcore
    at: 2026-10-02T08:37:07.889Z
    by: session:7c4c6f53
    who: Aleksandra-Mitricheva
---
On x-cart 3.1037.0-pr-141 introspection shows WishlistType has only the singular sharingSetting (isDeprecated false, no deprecationReason) and NO plural sharingSettings field. SharingSettingType.sharedWithId is isDeprecated true with deprecationReason 'Use targets'; the replacement is SharingSettingType.targets: [SharingTargetType] with id, name, subtitle, imageUrl (none deprecated). Input types InputCreateWishlistType and InputChangeWishlistType keep sharedWithId (not deprecated) beside addSharedWithIds, removeSharedWithIds and message.
