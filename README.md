# gummiebot: Gumtree Australia automation software

Get your ads, post new ads, and delete them automatically.

## Installation

    git clone https://github.com/mariuszskon/gummiebot
    cd gummiebot
    python -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

## Command line usage

    usage: gummiebot COMMAND DIRECTORY...
           Automation script for Gumtree Australia
           Execute COMMAND on one or more DIRECTORY sequentially

    COMMANDS
        post        Uploads ad
        delete      Deletes ad by name
        refresh     Finds and deletes ad, and then posts it,
                    failing if ad did not exist previously
        repost      Finds and deletes ad, if it exists,
                    and then posts ad

More than one directory can be provided. These will be processed one-by-one in order, but will only require logging in once.

Your directory must have a `meta.gummie.json` file describing the ad.

## meta.gummie.json

### Example

    {
        "title": "HDMI cables",
        "price": {
            "amount": 4,
            "type": "FIXED"
        },
        "condition": "new",
        "description_file": "desc",
        "category": "TV Accessories",
        "images": [
            "img01.jpg",
            "img02.jpg"
        ]
    }

### Required properties

#### `title`

The name of the ad, as a string.

#### `description_file`

A string which provides the filename of a pure text file relative to the directory of `meta.gummie.json`. This is used as the description on the Gumtree ad.

#### `price`

An object containing the properties `amount` and `type`.

##### `amount`

The asking price, in Australian dollars, provided via a type which can be converted to a float (decimal) easily. Must be greater than zero.

##### `type`

A string, one of `FIXED`, `NEGOTIABLE`, `GIVE_AWAY` or `SWAP_TRADE`, corresponding to the type of price on Gumtree.

#### `category`

A string which exactly matches one of the final categories on Gumtree. That is, this is a category which you can post under, not a broad category.

Yes: `Fiction Books`

No: `Books`

#### `images`

An array of strings, each providing the filename of an image relative to the directory of `meta.gummie.json`. Each image is uploaded to support the ad, in the order provided.

### Optional properties

#### `condition`

A string, one of `used` or `new`. Defaults to `used`.

## API usage

Undocumented functions/methods/classes are intended for internal use only.

### `gummie_json_parse(directory: str) -> GumtreeListing`

Changes to directory `directory` and reads the `meta.gummie.json` file inside. Returns an instance of `GumtreeListing` which contains validated data that can be used later with `GummieBot`.

### `GummieBot(username, password)`

Big class for interacting with Gumtree Australia, but not without logging in first.

#### `.category_map`

A dict of "leaf" categories (ones which ads can be submitted to), with mappings to their corresponding IDs. This is lazy loaded from the website.

#### `.ads`

Returns a dict of the logged in user's ads, with a mapping from the title to the ad's ID. This is lazy loaded from the website.

#### `.category_name_to_id(category_name)`

Provides a higher-level interface to reading a value using a key from `category_map` by logging if the key could not be found, and suggesting similar keys.

#### `.delete_ad_by_name(name) -> bool`

Delete an ad by its name/title. Returns boolean indicating success or failure.

#### `.delete_ad_by_id(id) -> bool`

Delete an ad by its ID. The ad ID can be acquired by first reading `.ads`. Returns boolean indicating success or failure. You might want to use `.detele_ad_by_name(name)` instead.

#### `.post_ad(ad: GumtreeListing) -> bool`

Posts an instance of `GumtreeListing` to Gumtree. Returns boolean indicating success or failure.

{'description': 'HMDI Cables in perfect condition.', 'price.amount': 4.0, 'price.type': 'FIXED', 'images': ['https://gumtreeau-res.cloudinary.com/image/private/t_$_74/gumtree/9f23e44c-3698-44b6-ae25-e7766379fae3.jpg', 'https://gumtreeau-res.cloudinary.com/image/private/t_$_74/gumtree/ec25351b-d0d9-453a-86a7-479811ea4585.jpg']}


[
    {
        "id": "20046_TEXT_FIELD_ad_title",
        "componentType": "TEXT_FIELD",
        "name": "title",
        "value": "hdmi cables"
    },
    {
        "id": "20046_CATEGORY_FIELD_ad_category",
        "componentType": "CATEGORY_FIELD",
        "name": "level2CategoryId",
        "value": "20046"
    },
    {
        "id": "20046_PRICE_FIELD_ad_price",
        "componentType": "PRICE_FIELD",
        "name": "price",
        "value": {
            "priceType": "FREE"
        }
    },
    {
        "id": "20046_SINGLE_SELECTION_FIELD_AD_TYPE",
        "componentType": "SINGLE_SELECTION_FIELD",
        "name": "adType",
        "value": "OFFER"
    },
    {
        "id": "20046_TEXT_AREA_ad_description",
        "componentType": "TEXT_AREA",
        "name": "description",
        "value": "HDMI Cables HDMI Cables HDMI Cables HDMI Cables"
    },
    {
        "id": "20046_BOOLEAN_FIELD_offer_shipping",
        "componentType": "BOOLEAN_FIELD",
        "name": "offerShipping",
        "value": false
    },
    {
        "id": "20046_SINGLE_SELECTION_FIELD_electronics_computers_other.condition_s",
        "componentType": "SINGLE_SELECTION_FIELD",
        "name": "attributeMap[electronics_computers_other.condition_s]",
        "value": "new"
    },
    {
        "id": "20046_TEXT_FIELD_contact_name",
        "componentType": "TEXT_FIELD",
        "name": "contactName",
        "value": "Cian"
    },
    {
        "id": "20046_LOCATION_FIELD_contact_location",
        "componentType": "LOCATION_FIELD",
        "name": "mapAddress",
        "value": {
            "mapAddress": "Australia",
            "locationId": "0",
            "longitude": 133.77513,
            "latitude": -25.27439,
            "confidenceLevel": "INVALID",
            "streetName": "",
            "houseNumber": "",
            "postcode": "",
            "localityName": "Australia",
            "showLocationOnMap": "false"
        }
    },
    {
        "id": "20046_TEXT_FIELD_phone_number",
        "componentType": "TEXT_FIELD",
        "name": "phoneNumber",
        "value": ""
    },
    {
        "id": "20046_BOOLEAN_FIELD_phone_number",
        "componentType": "BOOLEAN_FIELD",
        "name": "showPhoneOnVip",
        "value": false
    },
    {
        "id": "20046_TEXT_FIELD_link_website",
        "componentType": "TEXT_FIELD",
        "name": "website",
        "value": "",
        "featureType": "AD_PAID_URL"
    },
    {
        "id": "20046_BUNDLE_SELECTION_bundle_selection",
        "componentType": "BUNDLE_SELECTION",
        "name": "bundleSelection",
        "value": "PACKAGE_0"
    }
]