# Posting Image to Instagram with Caption

# Overview

Link: https://developers.facebook.com/docs/instagram-api/overview
The document is written for API version 14.0

Most Important thing to note first:

- Create Facebook Page
- Connect Facebook Page to Instagram Account

As Stated Officially:

```
Instagram Professional accounts are accessed indirectly through Facebook accounts so your app users must have a Facebook account and use it when signing into your app. In addition, the Facebook account must be able to perform admin-equivalent Tasks on a Facebook Page that has been connected to the Instagram account they are trying to access.
```

Link: https://developers.facebook.com/docs/instagram-api/overview#app-users

## Authentication

Official Statement:

```
App user authentication is handled through access tokens. Instagram Professional accounts are accessed indirectly through Facebook accounts, so all API requests must include your app users's Facebook User access token. You can get tokens from app users by implementing Facebook Login. Note that Facebook Login does not support Instagram credentials so app users must sign in using a Facebook account.
```

## Permission

Official Statement:

```
The API uses the following permissions and features:

instagram_basic
instagram_content_publish
instagram_manage_comments
instagram_manage_insights
instagram_shopping_tag_products
pages_show_list
pages_read_engagement
Instagram Public Content Access
```

## Pages

Official Statement:

```
Instagram Professional accounts must be connected to a Facebook Page before their data can be accessed through the API. Once connected, any Facebook User who is able to perform Tasks on that Page can grant your app an access token, which can then be used in API requests.

Our Add a Facebook Page to your Instagram Business Account help article explains how to connect to a Facebook Page to an Instagram Professional account.
```

# Publishing Content

Note: Please get `access token` in your custody before going through the doc.

## Get {ig-user-id}

### Get Pages List

```
https://graph.facebook.com/v14.0/me/accounts?access_token={access-token}
```

Output:

```
{
  "data": [
    {
      "access_token": "EAAJjmJ...",
      "category": "App Page",
      "category_list": [
        {
          "id": "2301",
          "name": "App Page"
        }
      ],
      "name": "Shameel_Page",
      "id": "134895793791915",  // capture the Page ID
      "tasks": [
        "ANALYZE",
        "ADVERTISE",
        "MODERATE",
        "CREATE_CONTENT",
        "MANAGE"
      ]
    }
  ]
}
```

**Make sure that your page is connected with Instagram**

## Get Instagram Business Account ID

```
https://graph.facebook.com/v14.0/101429795942358?fields=instagram_business_account&access_token={access-token}
```

Output:

```
{
  "instagram_business_account": {
    "id": "17841405822304915"  // Connected IG User ID
  },
  "id": "134895793791915"  // Facebook Page ID
}
```

## Post Image

Note: This is a **POST** request.

Sample:

```
https://graph.facebook.com/v14.0/{ig-user-id}/media?image_url={image-URL}&caption={test-caption}
```

Example:

```
https://graph.facebook.com/v14.0/17841405822304915/media?image_url=https://fbmeta.geeksroot.net/img/feed-1.jpg&caption=test-caption
```

Output:

```
{
    "id": "17941316888158286"
}
```

Note: This is a **creation ID**.
This needs to be inserted in next API

## Publish Post

Sample:

```
https://graph.facebook.com/v14.0/{ig-user-id}/media_publish?creation_id={creation-id}
```

Example:

```
https://graph.facebook.com/v14.0/17841453591775286/media_publish?creation_id=17941316888158286
```

Output:

```
{
    "id": "17843766734805742"
}
```

## Check Results:

Sample:

```
https://graph.facebook.com/v14.0/{ig-user-id}/media?access_token={access-token}
```

Example:

```
https://graph.facebook.com/v14.0/17841453591775286/media?access_token={example-token}
```

Output:

```
{
    "data": [
        {
            "id": "17843766734805742"
        }
    ],
    "paging": {
        "cursors": {
            "before": "QVFIUkZAwYlo0UjQ5cnh2V1JOSnVzUWVUZAG1qVy1JZAWZAsbjlkWklwYVlCUlFuaXQ5OGxpVDFHeXRnaTgteXpQX0hZAVHp5UGVRLVVkVEROMVJUSHFLd1VLNVdn",
            "after": "QVFIUjNyNzRFRDRDUkp5WWZAHRDgtZA3l2YVU3RG9BdlRlc3d5dVV4V3hVTEpFMGstSDdtcC0zQlQ2NGhlRFcxeXRBLVJpRjZAUV09obERjX0hKQ0tkcWctTmhn"
        }
    }
}
```
