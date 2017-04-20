---
title: API reference for Content Moderator  | Microsoft Docs
description: Learn about the Review API, Image and Text Moderation APIs, and List Management API for the Content Moderator.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.technology: content-moderator
ms.topic: article
ms.date: 02/19/2017
ms.author: sajagtap
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 5146fc8b55393dd73fdca8586d8a83fd08cc576f
ms.lasthandoff: 04/12/2017

---

# <a name="api-reference"></a>API Reference #

## <a name="review-api"></a>Review API ##
When you sign up for the [human review tool](http://contentmoderator.cognitive.microsoft.com/ "Content Moderator Review Tool"), you can use the team credentials, **Client Id** and **Client Secret** available within the **Credentials** TAB under the **Settings** section as shown below.

Also see: [How to authenticate your Review API calls](review-api-authentication.md).

![Content Moderator Review API Credentials](images/Moderator-Review-API-Credentials.PNG)

[**Review API Reference**](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5 "Content Moderator Review API")

## <a name="image-and-text-moderation-apis"></a>Image and Text Moderation APIs ##
You can get started on trying out the image and text moderation APIs in two ways:

  1. [Subscribe to the Content Moderator API](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/ContentModerator) on the Microsoft Azure portal.
  1. Sign up for the online [content moderator review tool](http://contentmoderator.cognitive.microsoft.com/).

If you sign up for the review tool, you will find your Content Moderator API keys listed on the **Connectors** TAB in the **Settings** section as shown below.

![Your Content Moderator API Key](images/Moderator-API-Key2.PNG)

The image and text moderation APIs scan your content and send the results with relevant information either back to your systems or to the built-in review tool, depending on who is calling the API. You can use this information to implement your own post-moderation workflow such as publish, reject, or review.

[**Image and Text Moderation APIs Reference**](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c "Image and Text Moderation APIs")

## <a name="list-management-api"></a>List Management API ##
You can use your Content Moderator API credentials (for image, text and reviews) for the list management API as well. Use this API to create and manage your custom lists of Images and Text. The lists that you create can be used in the **Image/Match** and **Text/Screen** operations.

[**List Management API Reference**](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f675 "Content Moderator List Management API")
