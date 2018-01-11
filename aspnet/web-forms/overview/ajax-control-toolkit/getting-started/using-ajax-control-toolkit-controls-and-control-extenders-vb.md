---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: "Za pomocą technologii AJAX kontroli zestawu narzędzi kontrolek i rozszerzeń formantu (VB) | Dokumentacja firmy Microsoft"
author: microsoft
description: "Dowiedz się, jak dodać kontrolki AJAX kontroli narzędzi i rozszerzeń do stron ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 7b248855a1b82f3e8f172b439ee36502f95a39ca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="5e32d-103">Za pomocą technologii AJAX kontroli zestawu narzędzi kontrolek i rozszerzeń formantu (VB)</span><span class="sxs-lookup"><span data-stu-id="5e32d-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="5e32d-104">przez [firmy Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5e32d-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5e32d-105">Dowiedz się, jak dodać kontrolki AJAX kontroli narzędzi i rozszerzeń do stron ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e32d-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="5e32d-106">Zestaw narzędzi kontroli AJAX zawiera zestaw kontrolek i rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="5e32d-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="5e32d-107">W tym samouczku krótkie możesz Dowiedz się, jak dodać kontrolki i rozszerzeń formantu strony ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e32d-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="5e32d-108">Aby uzyskać instrukcje na temat instalowania narzędzi kontroli AJAX i dodawanie Toolkit kontroli AJAX do przybornika programu Visual Studio/Visual Web Developer zobacz samouczek [wprowadzenie do zestawu narzędzi kontroli AJAX](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="5e32d-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="5e32d-109">Za pomocą technologii AJAX kontroli zestawu narzędzi formantów</span><span class="sxs-lookup"><span data-stu-id="5e32d-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="5e32d-110">Formant AJAX kontroli narzędzi działa tak, jak normalne formant ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e32d-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="5e32d-111">Można przeciągnij formant z przybornika do strony programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e32d-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="5e32d-112">Można dodać kontrolki na stronie w widoku projektu lub źródła.</span><span class="sxs-lookup"><span data-stu-id="5e32d-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="5e32d-113">Istnieje jedno wymaganie specjalne, gdy za pomocą formantów z zestawu narzędzi kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="5e32d-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="5e32d-114">Strona musi zawierać formantu ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="5e32d-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="5e32d-115">Formantu ScriptManager jest odpowiedzialny za tym wszystkie niezbędne języka JavaScript wymagane przez formanty Toolkit kontroli AJAX.</span><span class="sxs-lookup"><span data-stu-id="5e32d-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="5e32d-116">Na przykład karta AJAX kontroli Toolkit zawiera formant o nazwie kontrolce edytora.</span><span class="sxs-lookup"><span data-stu-id="5e32d-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="5e32d-117">Ten formant Wyświetla Zaawansowany edytor HTML.</span><span class="sxs-lookup"><span data-stu-id="5e32d-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="5e32d-118">Wykonaj następujące kroki, aby dodać kontrolki edytora do strony:</span><span class="sxs-lookup"><span data-stu-id="5e32d-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="5e32d-119">Utwórz nową stronę ASP.NET o nazwie ShowEditor.aspx</span><span class="sxs-lookup"><span data-stu-id="5e32d-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="5e32d-120">Wybierz formantu ScriptManager from beneath karta rozszerzenia AJAX w przyborniku i przeciągnij kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="5e32d-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="5e32d-121">Wybierz kontrolkę Edytor from beneath karcie Toolkit kontroli AJAX w przyborniku i przeciągnij kontrolki na stronie (zobacz rysunek 1).</span><span class="sxs-lookup"><span data-stu-id="5e32d-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="5e32d-122">Projektant powinien wyglądać na rysunku 2.</span><span class="sxs-lookup"><span data-stu-id="5e32d-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="5e32d-123">Uruchom witrynę sieci web, wybierając opcję menu **debugowania i Rozpocznij debugowanie** lub naciśnięcie klawisza F5.</span><span class="sxs-lookup"><span data-stu-id="5e32d-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="5e32d-124">Powinna zostać wyświetlona strona na rysunku 3.</span><span class="sxs-lookup"><span data-stu-id="5e32d-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="5e32d-125">[![Wybranie kontrolki edytora HTML](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="5e32d-126">**Rysunek 01**: Wybieranie kontrolce edytora HTML ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="5e32d-127">[![Projektanta Visual Studio za pomocą formantu ScriptManager i edytowanie](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="5e32d-128">**Rysunek 02**: projektanta Visual Studio za pomocą formantu ScriptManager i Edytuj ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="5e32d-129">[![Na stronie DisplayEditor.aspx](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="5e32d-130">**Rysunek 03**: DisplayEditor.aspx strony ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="5e32d-131">Przy użyciu rozszerzeń kontroli zestawu narzędzi kontroli AJAX</span><span class="sxs-lookup"><span data-stu-id="5e32d-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="5e32d-132">Zestaw narzędzi kontroli AJAX zawiera także rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="5e32d-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="5e32d-133">Zgodnie z sugestią, jego nazwa, rozszerzający kontroli rozszerza funkcjonalność formant.</span><span class="sxs-lookup"><span data-stu-id="5e32d-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="5e32d-134">Na przykład rozszerzeń formantu ConfirmButton rozszerza standardowe formantu przycisku ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5e32d-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="5e32d-135">Extender zmienia zachowanie s formantu przycisku Tak, aby przycisk wyświetla okno dialogowe potwierdzenia, gdy zostanie kliknięty.</span><span class="sxs-lookup"><span data-stu-id="5e32d-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="5e32d-136">Rozszerzeń formantu, podobnie jak kontroli zestawu narzędzi kontroli AJAX wymaga formantu ScriptManager.</span><span class="sxs-lookup"><span data-stu-id="5e32d-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="5e32d-137">Należy dodać formantu ScriptManager do strony przed rozpoczęciem korzystania z rozszerzeń kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="5e32d-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="5e32d-138">Wykonaj następujące kroki, aby użyć rozszerzeń formantu ConfirmButton:</span><span class="sxs-lookup"><span data-stu-id="5e32d-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="5e32d-139">Utwórz nową stronę ASP.NET o nazwie ShowConfirmButton.aspx</span><span class="sxs-lookup"><span data-stu-id="5e32d-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="5e32d-140">Dodawanie formantu ScriptManager na stronie przeciągając kontrolki na stronie from beneath karta rozszerzenia AJAX.</span><span class="sxs-lookup"><span data-stu-id="5e32d-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="5e32d-141">Dodawanie formantu standardowego przycisku do strony przeciągając przycisk from beneath standardową kartę w przyborniku na powierzchnię projektanta.</span><span class="sxs-lookup"><span data-stu-id="5e32d-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="5e32d-142">Kliknij przycisk **Dodawanie rozszerzeń** zadań opcji (zobacz rysunek 4).</span><span class="sxs-lookup"><span data-stu-id="5e32d-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="5e32d-143">W oknie dialogowym Wybieranie rozszerzeń, wybierz ConfirmButtonExtender (patrz rysunek 5) i kliknij przycisk OK.</span><span class="sxs-lookup"><span data-stu-id="5e32d-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="5e32d-144">Wybierz kontrolkę przycisku w projektancie, a następnie rozwiń węzeł rozszerzenia, Button1\_ConfirmButtonExtender węzeł w oknie właściwości (patrz rysunek 6).</span><span class="sxs-lookup"><span data-stu-id="5e32d-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="5e32d-145">Przypisuje wartość *naprawdę?* właściwości ConfirmText.</span><span class="sxs-lookup"><span data-stu-id="5e32d-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="5e32d-146">Uruchom strony przez wybranie opcji menu **debugowania i Rozpocznij debugowanie** lub naciśnij klawisz F5.</span><span class="sxs-lookup"><span data-stu-id="5e32d-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="5e32d-147">[![Opcja zadań Dodawanie rozszerzeń](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="5e32d-148">**Rysunek 04**: Dodawanie rozszerzeń zadań opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="5e32d-149">[![Wybieranie rozszerzeń formantu ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="5e32d-150">**Rysunek 05**: Wybieranie rozszerzeń formantu ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="5e32d-151">[![Ustawienie właściwości ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="5e32d-152">**Rysunek 06**: ustawienie właściwości ConfirmButton ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="5e32d-153">Po otwarciu strony, powinien być widoczny przycisk.</span><span class="sxs-lookup"><span data-stu-id="5e32d-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="5e32d-154">Po kliknięciu przycisku zostaną wyświetlone okno dialogowe potwierdzenia na rysunku 7.</span><span class="sxs-lookup"><span data-stu-id="5e32d-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="5e32d-155">[![Wyświetlanie okna dialogowego potwierdzenia](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="5e32d-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="5e32d-156">**Rysunek 07**: wyświetlanie okna dialogowego potwierdzenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="5e32d-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="5e32d-157">Zwróć uwagę, że zwykle nie przeciągania rozszerzeń kontrolki na stronie.</span><span class="sxs-lookup"><span data-stu-id="5e32d-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="5e32d-158">Zamiast tego należy użyć **Dodawanie rozszerzeń** zadań opcję, aby dodać rozszerzeń do formantu, który już został dodany do strony.</span><span class="sxs-lookup"><span data-stu-id="5e32d-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="5e32d-159">Zwróć uwagę, ponadto ustawienie sterowania właściwości rozszerzeń po otwarciu arkusza właściwości rozszerzanego formantu.</span><span class="sxs-lookup"><span data-stu-id="5e32d-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="5e32d-160">Jeden formant ASP.NET można rozszerzyć przez wiele rozszerzeń formantu.</span><span class="sxs-lookup"><span data-stu-id="5e32d-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="5e32d-161">Arkusz właściwości formantu rozszerzaną spowoduje wyświetlenie listy wszystkich rozszerzeń formantu skojarzony z formantem.</span><span class="sxs-lookup"><span data-stu-id="5e32d-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5e32d-162">[Poprzednie](get-started-with-the-ajax-control-toolkit-vb.md)
[dalej](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5e32d-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>