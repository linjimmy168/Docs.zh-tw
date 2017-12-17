---
uid: web-api/overview/security/external-authentication-services
title: "外部驗證服務與 ASP.NET Web 應用程式開發介面 (C#) |Microsoft 文件"
author: rmcmurray
description: "描述如何使用 ASP.NET Web API 中的外部驗證服務。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 5d6e6727f387d047e7b41a6efa0d2dadf467558e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="9ac1e-103">外部驗證服務與 ASP.NET Web 應用程式開發介面 (C#)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-103">External Authentication Services with ASP.NET Web API (C#)</span></span>
====================
<span data-ttu-id="9ac1e-104">由[Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="9ac1e-105">展開 visual Studio 2013 和 ASP.NET 4.5.1 的安全性選項[單一頁面應用程式](../../../single-page-application/index.md)(SPA) 和[Web API](../../index.md)與外部驗證服務，包括數個整合服務OAuth/OpenID 和社交媒體驗證服務： Microsoft 帳戶、 Twitter、 Facebook 和 Google。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-105">Visual Studio 2013 and ASP.NET 4.5.1 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>

### <a name="in-this-walkthrough"></a><span data-ttu-id="9ac1e-106">在本逐步解說</span><span class="sxs-lookup"><span data-stu-id="9ac1e-106">In This Walkthrough</span></span>

- [<span data-ttu-id="9ac1e-107">使用外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="9ac1e-107">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="9ac1e-108">建立 Web 應用程式範例</span><span class="sxs-lookup"><span data-stu-id="9ac1e-108">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="9ac1e-109">啟用 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-109">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="9ac1e-110">啟用 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-110">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="9ac1e-111">啟用 Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-111">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="9ac1e-112">啟用 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-112">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="9ac1e-113">其他資訊</span><span class="sxs-lookup"><span data-stu-id="9ac1e-113">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="9ac1e-114">結合外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="9ac1e-114">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="9ac1e-115">設定 IIS Express 要使用完整網域名稱</span><span class="sxs-lookup"><span data-stu-id="9ac1e-115">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="9ac1e-116">如何取得 Microsoft 驗證的應用程式設定</span><span class="sxs-lookup"><span data-stu-id="9ac1e-116">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="9ac1e-117">選擇性： 停用本機註冊</span><span class="sxs-lookup"><span data-stu-id="9ac1e-117">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="9ac1e-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="9ac1e-118">Prerequisites</span></span>

<span data-ttu-id="9ac1e-119">若要依照本逐步解說中的範例，您需要具備以下項目：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-119">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="9ac1e-120">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="9ac1e-120">Visual Studio 2013</span></span>
- <span data-ttu-id="9ac1e-121">至少一個下列外部驗證服務帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-121">An account for at least one of the following external authentication services:</span></span>

    - <span data-ttu-id="9ac1e-122">Google 使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="9ac1e-122">A Google user account</span></span>
    - <span data-ttu-id="9ac1e-123">使用應用程式識別碼和祕密金鑰，如以下的社交媒體驗證服務的其中一個開發人員帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-123">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

        - <span data-ttu-id="9ac1e-124">Microsoft 帳戶 ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="9ac1e-124">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
        - <span data-ttu-id="9ac1e-125">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="9ac1e-125">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
        - <span data-ttu-id="9ac1e-126">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="9ac1e-126">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="9ac1e-127">使用外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="9ac1e-127">Using External Authentication Services</span></span>

<span data-ttu-id="9ac1e-128">若要減少程式開發的 web 開發人員說明目前可用的外部驗證服務的眾多時建立新的 web 應用程式的時間。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-128">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="9ac1e-129">Web 使用者時通常會有數個現有的帳戶以常用的 web 服務和社交媒體網站，因此從外部 web 服務或網站社交媒體服務驗證 web 應用程式實作，它會將儲存的開發時間，將已使建立驗證實作。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-129">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="9ac1e-130">使用外部驗證服務會儲存使用者不必建立另一個帳戶用於您的 web 應用程式，而且也不需要記住另一個使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-130">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="9ac1e-131">在過去，開發人員有兩個選擇： 建立自己的驗證實作，或了解如何將外部驗證服務整合至其應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-131">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="9ac1e-132">在最基本的層級下, 圖說明簡單的要求流程使用者代理程式 （web 瀏覽器） 設定為使用外部驗證服務的 web 應用程式要求資訊：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-132">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

<span data-ttu-id="9ac1e-133">[![](external-authentication-services/_static/image2.png "按一下以展開映像")](external-authentication-services/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-133">[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)</span></span>

<span data-ttu-id="9ac1e-134">在上圖中，使用者代理程式 （或在此範例中的網頁瀏覽器） 會提出要求的 web 應用程式，重新導向至外部驗證服務的網頁瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-134">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="9ac1e-135">使用者代理程式會將憑證傳送至外部驗證服務，並已成功驗證的使用者代理程式，如果外部驗證服務重新導向使用者代理程式至原始的 web 應用程式，使用某種形式的語彙基元的使用者代理程式會將傳送至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-135">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="9ac1e-136">Web 應用程式會使用語彙基元確認使用者代理程式已成功驗證外部驗證服務，而且 web 應用程式可能會使用權杖來收集使用者代理程式的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-136">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="9ac1e-137">完成應用程式正在處理使用者代理程式的資訊，web 應用程式會傳回適當的回應取決於其授權設定的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-137">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="9ac1e-138">在第二個範例中，使用者代理程式與 web 應用程式和外部授權伺服器交涉和 web 應用程式會執行其他與外部授權伺服器通訊來擷取有關使用者的其他資訊代理程式：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-138">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

<span data-ttu-id="9ac1e-139">[![](external-authentication-services/_static/image4.png "按一下以展開映像")](external-authentication-services/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-139">[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)</span></span>

<span data-ttu-id="9ac1e-140">Visual Studio 2013 和 ASP.NET 4.5.1 與外部驗證服務的整合更輕鬆地進行開發人員藉由提供內建整合，如以下驗證服務：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-140">Visual Studio 2013 and ASP.NET 4.5.1 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="9ac1e-141">Facebook</span><span class="sxs-lookup"><span data-stu-id="9ac1e-141">Facebook</span></span>
- <span data-ttu-id="9ac1e-142">Google</span><span class="sxs-lookup"><span data-stu-id="9ac1e-142">Google</span></span>
- <span data-ttu-id="9ac1e-143">Microsoft 帳戶 （Windows Live ID 帳戶）</span><span class="sxs-lookup"><span data-stu-id="9ac1e-143">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="9ac1e-144">Twitter</span><span class="sxs-lookup"><span data-stu-id="9ac1e-144">Twitter</span></span>

<span data-ttu-id="9ac1e-145">在本逐步解說的範例將示範如何使用 Visual Studio 2013 中使用新的 ASP.NET Web 應用程式範本所隨附設定每個支援的外部驗證服務。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-145">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2013.</span></span>

> [!NOTE]
> <span data-ttu-id="9ac1e-146">如有必要，您可能需要將您的 FQDN 新增到您的外部驗證服務的設定。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-146">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="9ac1e-147">這項需求根據需要應用程式設定中的 FQDN，以符合您的用戶端會使用 FQDN 某些外部驗證服務的安全性限制。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-147">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="9ac1e-148">（這個步驟會有極大不同，為每個外部驗證服務，您必須參閱說明文件以查看是否需要為每個外部驗證服務，以及如何設定這些設定）。如果您要設定 IIS Express 來測試此環境中使用 FQDN，請參閱[使用完整網域名稱設定 IIS Express](#FQDN)稍後在本逐步解說 > 一節。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-148">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a><span data-ttu-id="9ac1e-149">建立範例 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9ac1e-149">Creating a Sample Web Application</span></span>

<span data-ttu-id="9ac1e-150">下列步驟將引導您完成範例應用程式使用 ASP.NET Web 應用程式範本建立，並將此範例應用程式對每個外部驗證服務，稍後在本逐步解說。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-150">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="9ac1e-151">啟動 Visual Studio 2013 選取**新專案**從 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-151">Start Visual Studio 2013 select **New Project** from the Start page.</span></span> <span data-ttu-id="9ac1e-152">或從**檔案**功能表上，選取**新增**然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-152">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="9ac1e-153">[![](external-authentication-services/_static/image6.png "按一下以展開映像")](external-authentication-services/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-153">[![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png)</span></span>

<span data-ttu-id="9ac1e-154">當**新專案**對話方塊隨即出現，請選取**已安裝****範本**展開**Visual C#**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-154">When the **New Project** dialog box is displayed, select **Installed** **Templates** and expand **Visual C#**.</span></span> <span data-ttu-id="9ac1e-155">在下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-155">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="9ac1e-156">在專案範本清單中選取**ASP.NET Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-156">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="9ac1e-157">輸入您的專案名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-157">Enter a name for your project and click **OK**.</span></span>

<span data-ttu-id="9ac1e-158">[![](external-authentication-services/_static/image8.png "按一下以展開映像")](external-authentication-services/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-158">[![](external-authentication-services/_static/image8.png "Click to Expand the Image")](external-authentication-services/_static/image7.png)</span></span>

<span data-ttu-id="9ac1e-159">當**新增 ASP.NET 專案**顯示，請選取**SPA**範本，然後按一下**建立專案**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-159">When the **New ASP.NET Project** is displayed, select the **SPA** template and click **Create Project**.</span></span>

<span data-ttu-id="9ac1e-160">[![](external-authentication-services/_static/image10.png "按一下以展開映像")](external-authentication-services/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-160">[![](external-authentication-services/_static/image10.png "Click to Expand the Image")](external-authentication-services/_static/image9.png)</span></span>

<span data-ttu-id="9ac1e-161">等候與 Visual Studio 2013 中建立您的專案。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-161">Wait as Visual Studio 2013 creates your project.</span></span>

<span data-ttu-id="9ac1e-162">[![](external-authentication-services/_static/image12.png "按一下以展開映像")](external-authentication-services/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-162">[![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png)</span></span>

<span data-ttu-id="9ac1e-163">Visual Studio 2013 已完成建立您的專案，開啟*Startup.Auth.cs*檔案位於**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-163">When Visual Studio 2013 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="9ac1e-164">[![](external-authentication-services/_static/image14.png "按一下以展開映像")](external-authentication-services/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-164">[![](external-authentication-services/_static/image14.png "Click to Expand the Image")](external-authentication-services/_static/image13.png)</span></span>

<span data-ttu-id="9ac1e-165">當您第一次建立專案時，沒有任何外部驗證服務中啟用*Startup.Auth.cs*檔案; 以下將說明您的程式碼可能類似、 反白顯示的位置區段會啟用外部驗證服務，以及任何相關的設定，才能搭配 ASP.NET 應用程式使用 Microsoft 帳戶、 Twitter、 Facebook 或 Google 驗證：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-165">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="9ac1e-166">當您按 F5 以建置並偵錯 web 應用程式時，它會顯示，您會看到已定義任何外部驗證服務的登入畫面。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-166">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

<span data-ttu-id="9ac1e-167">[![](external-authentication-services/_static/image16.png "按一下以展開映像")](external-authentication-services/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-167">[![](external-authentication-services/_static/image16.png "Click to Expand the Image")](external-authentication-services/_static/image15.png)</span></span>

<span data-ttu-id="9ac1e-168">在下列章節中，您將學習如何啟用每個外部驗證服務所提供的 Visual Studio 2013 中的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-168">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2013.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="9ac1e-169">啟用 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-169">Enabling Facebook Authentication</span></span>

<span data-ttu-id="9ac1e-170">使用 Facebook 驗證會要求您建立 Facebook 開發人員帳戶，且您的專案需要應用程式識別碼和來自 Facebook 的祕密金鑰才能運作。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-170">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="9ac1e-171">如需建立 Facebook 開發人員帳戶，並取得您的應用程式識別碼和祕密金鑰資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-171">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="9ac1e-172">一旦您取得您的應用程式識別碼和祕密金鑰，請使用下列步驟啟用 Facebook 驗證 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-172">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="9ac1e-173">在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-173">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="9ac1e-174">[![](external-authentication-services/_static/image18.png "按一下以展開映像")](external-authentication-services/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-174">[![](external-authentication-services/_static/image18.png "Click to Expand the Image")](external-authentication-services/_static/image17.png)</span></span>
2. <span data-ttu-id="9ac1e-175">找出反白顯示程式碼區段：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-175">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="9ac1e-176">移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，然後加入 您的應用程式識別碼和祕密金鑰。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-176">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="9ac1e-177">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-177">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="9ac1e-178">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Facebook 具有已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-178">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    <span data-ttu-id="9ac1e-179">[![](external-authentication-services/_static/image20.png "按一下以展開映像")](external-authentication-services/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-179">[![](external-authentication-services/_static/image20.png "Click to Expand the Image")](external-authentication-services/_static/image19.png)</span></span>
5. <span data-ttu-id="9ac1e-180">當您按一下**Facebook**  按鈕，您的瀏覽器會被重新導向至 Facebook 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-180">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    <span data-ttu-id="9ac1e-181">[![](external-authentication-services/_static/image22.png "按一下以展開映像")](external-authentication-services/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-181">[![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)</span></span>
6. <span data-ttu-id="9ac1e-182">您輸入您的 Facebook 認證，然後按一下之後**登入**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名稱**您想要關聯您Facebook 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-182">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    <span data-ttu-id="9ac1e-183">[![](external-authentication-services/_static/image24.png "按一下以展開映像")](external-authentication-services/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-183">[![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)</span></span>
7. <span data-ttu-id="9ac1e-184">您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Facebook 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-184">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    <span data-ttu-id="9ac1e-185">[![](external-authentication-services/_static/image26.png "按一下以展開映像")](external-authentication-services/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-185">[![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)</span></span>

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="9ac1e-186">啟用 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-186">Enabling Google Authentication</span></span>

<span data-ttu-id="9ac1e-187">Google 是到目前為止最簡單的啟用，因為它不需要開發人員帳戶，也不會要求您應用程式識別碼或密碼金鑰為其他外部驗證服務的其他資訊的外部驗證服務也許。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-187">Google is by far the easiest of the external authentication services to enable because it doesn't require a developer account, nor does it require additional information like your application ID or secret key as the other external authentication services necessitate.</span></span>

<span data-ttu-id="9ac1e-188">若要啟用 Google 驗證 web 應用程式，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-188">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="9ac1e-189">在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-189">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="9ac1e-190">[![](external-authentication-services/_static/image28.png "按一下以展開映像")](external-authentication-services/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-190">[![](external-authentication-services/_static/image28.png "Click to Expand the Image")](external-authentication-services/_static/image27.png)</span></span>
2. <span data-ttu-id="9ac1e-191">找出反白顯示程式碼區段：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-191">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="9ac1e-192">移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，再重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-192">Remove the &quot;//&quot; characters to uncomment the highlighted line of code, and then recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="9ac1e-193">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Google 具有已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-193">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    <span data-ttu-id="9ac1e-194">[![](external-authentication-services/_static/image30.png "按一下以展開映像")](external-authentication-services/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-194">[![](external-authentication-services/_static/image30.png "Click to Expand the Image")](external-authentication-services/_static/image29.png)</span></span>
5. <span data-ttu-id="9ac1e-195">當您按一下**Google**  按鈕，您的瀏覽器會被重新導向至 Google 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-195">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    <span data-ttu-id="9ac1e-196">[![](external-authentication-services/_static/image32.png "按一下以展開映像")](external-authentication-services/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-196">[![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)</span></span>
6. <span data-ttu-id="9ac1e-197">您輸入您的 Google 認證，然後按一下之後**登入**，Google 將會提示您確認您的 web 應用程式具有存取您的 Google 帳戶的權限：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-197">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    <span data-ttu-id="9ac1e-198">[![](external-authentication-services/_static/image34.png "按一下以展開映像")](external-authentication-services/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-198">[![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)</span></span>
7. <span data-ttu-id="9ac1e-199">當您按一下**接受**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名**您想要與您的 Google 帳戶產生關聯：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-199">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    <span data-ttu-id="9ac1e-200">[![](external-authentication-services/_static/image36.png "按一下以展開映像")](external-authentication-services/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-200">[![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)</span></span>
8. <span data-ttu-id="9ac1e-201">您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Google 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-201">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    <span data-ttu-id="9ac1e-202">[![](external-authentication-services/_static/image38.png "按一下以展開映像")](external-authentication-services/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-202">[![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)</span></span>

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="9ac1e-203">啟用 Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-203">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="9ac1e-204">Microsoft 驗證需要您建立開發人員帳戶，而且需要用戶端識別碼和用戶端密碼才能運作。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-204">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="9ac1e-205">如需建立 Microsoft 開發人員帳戶，並取得您的用戶端識別碼和用戶端密碼資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-205">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="9ac1e-206">一旦您取得您的取用者索引鍵和取用者密碼，請使用下列步驟啟用 web 應用程式的 Microsoft 驗證：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-206">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="9ac1e-207">在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-207">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="9ac1e-208">[![](external-authentication-services/_static/image40.png "按一下以展開映像")](external-authentication-services/_static/image39.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-208">[![](external-authentication-services/_static/image40.png "Click to Expand the Image")](external-authentication-services/_static/image39.png)</span></span>
2. <span data-ttu-id="9ac1e-209">找出反白顯示程式碼區段：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-209">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="9ac1e-210">移除&quot; // &quot;反白顯示的行程式碼，請取消註解，然後加入 您的用戶端識別碼和用戶端密碼的字元。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-210">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="9ac1e-211">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-211">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="9ac1e-212">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Microsoft 具有已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-212">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    <span data-ttu-id="9ac1e-213">[![](external-authentication-services/_static/image42.png "按一下以展開映像")](external-authentication-services/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-213">[![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)</span></span>
5. <span data-ttu-id="9ac1e-214">當您按一下**Microsoft**  按鈕，您的瀏覽器會被重新導向至 Microsoft 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-214">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    <span data-ttu-id="9ac1e-215">[![](external-authentication-services/_static/image44.png "按一下以展開映像")](external-authentication-services/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-215">[![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)</span></span>
6. <span data-ttu-id="9ac1e-216">您輸入您的 Microsoft 認證，然後按一下之後**登入**，會提示您確認您的 web 應用程式具有存取您的 Microsoft 帳戶的權限：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-216">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    <span data-ttu-id="9ac1e-217">[![](external-authentication-services/_static/image46.png "按一下以展開映像")](external-authentication-services/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-217">[![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)</span></span>
7. <span data-ttu-id="9ac1e-218">當您按一下**是**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名**您想要與您的 Microsoft 帳戶產生關聯：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-218">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    <span data-ttu-id="9ac1e-219">[![](external-authentication-services/_static/image48.png "按一下以展開映像")](external-authentication-services/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-219">[![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)</span></span>
8. <span data-ttu-id="9ac1e-220">您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**您的 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-220">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    <span data-ttu-id="9ac1e-221">[![](external-authentication-services/_static/image50.png "按一下以展開映像")](external-authentication-services/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-221">[![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)</span></span>

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="9ac1e-222">啟用 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="9ac1e-222">Enabling Twitter Authentication</span></span>

<span data-ttu-id="9ac1e-223">Twitter 驗證需要您建立開發人員帳戶，而且需要取用者索引鍵和取用者秘密才能運作。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-223">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="9ac1e-224">如需建立 Twitter 開發人員帳戶，並取得您的取用者索引鍵和取用者秘密資訊，請參閱[https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-224">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="9ac1e-225">一旦您取得您的取用者索引鍵和取用者密碼，請使用下列步驟啟用 Twitter 驗證 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-225">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="9ac1e-226">在 Visual Studio 2013 中開啟您的專案時，開啟*Startup.Auth.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-226">When your project is open in Visual Studio 2013, open the *Startup.Auth.cs* file:</span></span>

    <span data-ttu-id="9ac1e-227">[![](external-authentication-services/_static/image52.png "按一下以展開映像")](external-authentication-services/_static/image51.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-227">[![](external-authentication-services/_static/image52.png "Click to Expand the Image")](external-authentication-services/_static/image51.png)</span></span>
2. <span data-ttu-id="9ac1e-228">找出反白顯示程式碼區段：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-228">Locate the highlighted section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="9ac1e-229">移除&quot; // &quot;反白顯示的行程式碼，請取消註解，然後加入 您的取用者索引鍵和取用者密碼的字元。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-229">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="9ac1e-230">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-230">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="9ac1e-231">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Twitter 具有已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-231">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    <span data-ttu-id="9ac1e-232">[![](external-authentication-services/_static/image54.png "按一下以展開映像")](external-authentication-services/_static/image53.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-232">[![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)</span></span>
5. <span data-ttu-id="9ac1e-233">當您按一下**Twitter**  按鈕，您的瀏覽器會被重新導向至 Twitter 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-233">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    <span data-ttu-id="9ac1e-234">[![](external-authentication-services/_static/image56.png "按一下以展開映像")](external-authentication-services/_static/image55.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-234">[![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)</span></span>
6. <span data-ttu-id="9ac1e-235">您輸入您的 Twitter 認證，然後按一下之後**授權應用程式**，網頁瀏覽器會重新導向回 web 應用程式，這將會提示您輸入**使用者名稱**您想要與建立關聯您的 Twitter 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-235">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    <span data-ttu-id="9ac1e-236">[![](external-authentication-services/_static/image58.png "按一下以展開映像")](external-authentication-services/_static/image57.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-236">[![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)</span></span>
7. <span data-ttu-id="9ac1e-237">您輸入使用者名稱並按下之後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**Twitter 帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-237">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    <span data-ttu-id="9ac1e-238">[![](external-authentication-services/_static/image60.png "按一下以展開映像")](external-authentication-services/_static/image59.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-238">[![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)</span></span>

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="9ac1e-239">其他資訊</span><span class="sxs-lookup"><span data-stu-id="9ac1e-239">Additional Information</span></span>

<span data-ttu-id="9ac1e-240">如需有關建立使用 OAuth 和 OpenID 應用程式的詳細資訊，請參閱下列 Url:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-240">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [<span data-ttu-id="9ac1e-241">https://go.microsoft.com/fwlink/?LinkID=252166</span><span class="sxs-lookup"><span data-stu-id="9ac1e-241">https://go.microsoft.com/fwlink/?LinkID=252166</span></span>](https://go.microsoft.com/fwlink/?LinkID=252166)
- [<span data-ttu-id="9ac1e-242">https://go.microsoft.com/fwlink/?LinkID=243995</span><span class="sxs-lookup"><span data-stu-id="9ac1e-242">https://go.microsoft.com/fwlink/?LinkID=243995</span></span>](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="9ac1e-243">結合外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="9ac1e-243">Combining External Authentication Services</span></span>

<span data-ttu-id="9ac1e-244">較大的彈性，您可以同時定義多個外部驗證服務-這可讓您的 web 應用程式的使用者，以使用從任何已啟用的外部驗證服務的帳戶：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-244">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

<span data-ttu-id="9ac1e-245">[![](external-authentication-services/_static/image62.png "按一下以展開映像")](external-authentication-services/_static/image61.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-245">[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)</span></span>

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="9ac1e-246">設定 IIS Express 要使用完整網域名稱</span><span class="sxs-lookup"><span data-stu-id="9ac1e-246">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>

<span data-ttu-id="9ac1e-247">某些外部驗證提供者不支援測試您的應用程式使用的 HTTP 地址如下`http://localhost:port/`。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-247">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="9ac1e-248">若要暫時解決此問題，您可以新增靜態的完整格式網域名稱 (FQDN) 對應至主機檔案，並使用 FQDN 進行測試/偵錯 Visual Studio 2013 中設定您的專案選項。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-248">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2013 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="9ac1e-249">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-249">To do so, use the following steps:</span></span>

- <span data-ttu-id="9ac1e-250">新增靜態 FQDN，對應的主機檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-250">Add a static FQDN mapping your HOSTS file:</span></span>

    1. <span data-ttu-id="9ac1e-251">在視窗中開啟提升權限的命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-251">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="9ac1e-252">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-252">Type the following command:</span></span>

        <span data-ttu-id="9ac1e-253"><kbd>在 [記事本] %WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="9ac1e-253"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
    3. <span data-ttu-id="9ac1e-254">將類似下列的項目加入至主機檔案：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-254">Add an entry like the following to the HOSTS file:</span></span>

        <span data-ttu-id="9ac1e-255"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="9ac1e-255"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
    4. <span data-ttu-id="9ac1e-256">儲存並關閉主機檔案。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-256">Save and close your HOSTS file.</span></span>
- <span data-ttu-id="9ac1e-257">設定 Visual Studio 專案以使用 FQDN:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-257">Configure your Visual Studio project to use the FQDN:</span></span>

    1. <span data-ttu-id="9ac1e-258">在 Visual Studio 2013 中開啟您的專案時，請按一下**專案**功能表，然後選取您的專案屬性。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-258">When your project is open in Visual Studio 2013, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="9ac1e-259">例如，您可能會選取**WebApplication1 屬性**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-259">For example, you might select **WebApplication1 Properties**.</span></span>
    2. <span data-ttu-id="9ac1e-260">選取**Web**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-260">Select the **Web** tab.</span></span>
    3. <span data-ttu-id="9ac1e-261">輸入的 FQDN**專案 Url**。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-261">Enter your FQDN for the **Project Url**.</span></span> <span data-ttu-id="9ac1e-262">例如，您會輸入<kbd>http://www.wingtiptoys.com</kbd>如果這是您加入至主機檔案的 FQDN 對應。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-262">For example, you would enter <kbd>http://www.wingtiptoys.com</kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>
- <span data-ttu-id="9ac1e-263">設定 IIS Express 應用程式使用 FQDN:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-263">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="9ac1e-264">在視窗中開啟提升權限的命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-264">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="9ac1e-265">輸入下列命令以變更您的 IIS Express 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-265">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="9ac1e-266"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="9ac1e-266"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="9ac1e-267">輸入下列命令以將 FQDN 新增到您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-267">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="9ac1e-268"><kbd>appcmd.exe 設定組態-section:system.applicationHost/sites / +&quot;[名稱 = 'WebApplication1'].bindings。 [通訊協定 = 'http'，bindingInformation ='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="9ac1e-268"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

 <span data-ttu-id="9ac1e-269">其中**WebApplication1**是您的專案名稱和**bindingInformation**包含您想要用於測試的通訊埠編號和 FQDN。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-269">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="9ac1e-270">如何取得 Microsoft 驗證的應用程式設定</span><span class="sxs-lookup"><span data-stu-id="9ac1e-270">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="9ac1e-271">連結至 Windows Live Microsoft 驗證的應用程式是簡單的程序。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-271">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="9ac1e-272">如果您未連結至 Windows Live 應用程式，您可以使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-272">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="9ac1e-273">瀏覽至[https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070)並輸入您的 Microsoft 帳戶名稱和密碼提示時，然後按一下**登入**:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-273">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

    <span data-ttu-id="9ac1e-274">[![](external-authentication-services/_static/image64.png "按一下以展開映像")](external-authentication-services/_static/image63.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-274">[![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png)</span></span>
2. <span data-ttu-id="9ac1e-275">輸入的名稱和出現提示時，您的應用程式的語言，然後按一下**我接受**:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-275">Enter the name and language of your application when prompted, and then click **I accept**:</span></span>

    <span data-ttu-id="9ac1e-276">[![](external-authentication-services/_static/image66.png "按一下以展開映像")](external-authentication-services/_static/image65.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-276">[![](external-authentication-services/_static/image66.png "Click to Expand the Image")](external-authentication-services/_static/image65.png)</span></span>
3. <span data-ttu-id="9ac1e-277">上**API 設定**應用程式頁面上，輸入您的應用程式和複製的重新導向網域**用戶端識別碼**和**用戶端密碼**專案，然後按一下按一下**儲存**:</span><span class="sxs-lookup"><span data-stu-id="9ac1e-277">On the **API Settings** page for your application, enter the redirect domain for your application and copy the **Client ID** and **Client secret** for your project, and then click **Save**:</span></span>

    <span data-ttu-id="9ac1e-278">[![](external-authentication-services/_static/image68.png "按一下以展開映像")](external-authentication-services/_static/image67.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-278">[![](external-authentication-services/_static/image68.png "Click to Expand the Image")](external-authentication-services/_static/image67.png)</span></span>

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="9ac1e-279">選擇性： 停用本機註冊</span><span class="sxs-lookup"><span data-stu-id="9ac1e-279">Optional: Disable Local Registration</span></span>

<span data-ttu-id="9ac1e-280">目前 ASP.NET 本機註冊功能不會防止自動的程式 (bot) 建立成員的帳戶。例如，藉由使用 bot 防止和驗證技術，例如[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-280">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="9ac1e-281">因為這個緣故，您應該移除登入頁面上的本機登入表單和註冊連結。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-281">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="9ac1e-282">若要這樣做，請開啟 *\_Login.cshtml*在專案中，頁面上，然後再標記為註解的線本機登入面板和註冊連結。</span><span class="sxs-lookup"><span data-stu-id="9ac1e-282">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="9ac1e-283">結果頁面應該類似下列的程式碼範例類似：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-283">The resulting page should like like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="9ac1e-284">本機登入面板和註冊連結已停用，一旦您登入頁面只會顯示您已啟用外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="9ac1e-284">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

<span data-ttu-id="9ac1e-285">[![](external-authentication-services/_static/image70.png "按一下以展開映像")](external-authentication-services/_static/image69.png)</span><span class="sxs-lookup"><span data-stu-id="9ac1e-285">[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)</span></span>