---
parser: v2
author_name: Daniel Wroblewski
author_profile: https://github.com/thecodester
auto_validation: true
time: 10
tags: [ tutorial>beginner, software-product>sap-business-technology-platform,software-product>sap-build, software-product>sap-build-apps--enterprise-edition, software-product>sap-build-process-automation]
primary_tag: software-product>sap-build
---
  
 
# Enable App to Select Product by Scanning Barcode 
<!-- description --> Show some of the native mobile capabilities of SAP Build Apps by allowing users to select a product by scanning a barcode, as part of the SAP Build CodeJam. 



## Prerequisites
- You have completed the previous tutorial for the SAP Build CodeJam, [Add Rework Flow to Your Process](codejam-10-spa-rework).





## You will learn
- How to get and set up the Mobile Preview App
- How to enable barcode scanning




## Intro
One of the best things about SAP Build Apps is that you can easily enable basic but powerful native mobile features – such as the native option component or the photo selector or the camera or access to the contacts. 

In this tutorial, we will let you scan a barcode in order to select a product, instead of having to select from the list on the home page.

>**IMPORTANT:** Currently, it is not possible to get the current user when running the app on a mobile device (this is expected to be enabled in Q1 of 2024, but no guarantees). In the app, we use the current user's email to identify the user and indicate which order's in the CAP service belong to the current user.

>In order to enable our scenario to work on mobile, we will change the way we identify the current user.






### Install preview app on your device
To get and set up the SAP Build Apps Mobile Preview App, watch and follow along with this video from **Daniel Wroblewski**. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/UvdCV4-kGAY" frameborder="0" allowfullscreen></iframe> 







### Change customer logic
Because the mobile preview does not yet support getting user information, you will select a string and hardcode it as your user ID.

1. Go to the **Home page**, and toggle to **Variables**.

2. Create an app variable called **currentUser**.

    Set its **Variable value type** to **Text**.

    Set it's **Initial value** to some unique value, like your email address or some GUD (or you can create a GUID from some online generator, like [this one](https://guidgenerator.com/)).

3. Open the logic canvas.

4. Click on the **Get record collection**, and in the **Filter condition**, change the binding for **customer** condition.

    Set it to **Data and Variables > App variable > current user**.

5. Click on the **Create record**, and in the **Record** property, change the formula binding to the following formula.
   
    ```JavaScript
    {customer: appVars.currentUser, status: "CART"}
    ```

6. Click **Save** (upper right).






### Add UI for barcode scanning
1. In the UI, instead of the title component, replace that with the following:

    - Row
      - Cell #1
        - Title (this is the original title you had ... just drag it to this Cell)
      - Cell #2
        - Container
          - Text
          - Icon

    ![Change title](1-ChangeTitle.png)

    >When you drag in a row component, it will automatically contain 2 cells.

    The UI will now look like this.

    ![UI change](1-change-title-ui.png)

2. Select the row component (use the tree view), and in the **Layout** tab click the first **50%**.

    ![Alt text](1-row-1.png)

    Set the cell width to `5`.

    ![Cell width](1-row-2.png)

    Now click the other side, **0%**, and then enter for the cell width `3`.

    ![Cell width, again](1-row-3.png)

    This now sets the relative widths of the cells.

    ![Row widths](1-row-4.png)

    Finally, set **Align cell content** to align bottoms.

3. Select the container in Cell #2, and under **Layout**, set the layout to **Horizontal**.

    ![Container layout](1-container.png)

3. Select the text component (in Cell #2) and set the following.

    - Under **Properties**, set **Content** to `Click to scan product`.

        ![Text](1-text-1.png)

    - Under **Style**, click the **Primary Paragraph** dropdown, and click **Edit**.
        
        ![Edit style](1-text-2.png)

        Under **Typography**, set **Text size** to **Small text**.

    - Under **Layout**, change **Text align** to **right**.

4. Select the icon component (in Cell #2), and set the following.

    - Under **Properties**, set **Icon** to `barcode`.

        ![Icon](1-icon-1.png)

    - Under **Style**, click the **Primary Paragraph** dropdown, and click **Edit**.

        ![Style edit](1-icon-2.png)

        Under **Typography**, change the **Font size** to `60` (change the binding to a static number).

Your UI should now look like this:

![Final UI](1-final.png)





### Add logic to scan barcode
Now lets create the logic for handling the tap of the barcode icon.

1. Select the icon component, open the logic canvas.

    ![Open icon logic](2-openlogic.png)

2. Create the following logic, and connect the flow functions as shown. 

    ![Basic logic](2-basic-logic.png)

    >The logic opens the barcode scanner, and if the user actually selects a barcode, then we test that value.
    >
    >- If the value is a product number and in our list of products – held in our data variable – then we navigate to the product details page.
    >
    >- If the value is not a product, we just alert the user.

3. For the **If condition** flow function, set the **Condition** to the following formula:

    ```JavaScript
    IS_IN_ARRAY_BY_KEY(data.Products1, "Id",outputs["Scan QR/barcode"].codeContent)
    ```

4. For the **Open page** flow function, set the **Page** to **Product Details**.

    ![Open product details](2-openpage.png)

    Set the **productID** to the **Output value of another node > Scan QR/barcode > QR barcode content**.

5. For the **Alert** flow function, set **Dialog title** to the following formula:

    ```JavaScript
    "No such product " + outputs["Scan QR/barcode"].codeContent
    ```

The logic should now look like this:

![Final logic](2-final.png)






### Test with preview app
Now we will use the mobile preview app so we can use the native functionality of the device.

>Make sure you have installed the preview app on your device.
>
>- [Instructions for iOS](https://apps.apple.com/us/app/sap-appgyver-preview/id1585856868)
>
>- [Instructions for Android](https://play.google.com/store/apps/details?id=com.sap.appgyver.preview.release)



1. Click the **Launch** tab, and then **Open preview portal**.

    ![Open launch tab](3-preview-1.png)

    You should now get a place to enter a code from your preview app.

    ![Preview portal](3-preview-1a.png)

2. Open the preview app on your device.

3. Follow one of the following:

    - **For US10:** If you are on a trial account, likely you are on an SAP BTP account in the US10 region.

        - Click **Other login options**.

        - Select **SAP Build Apps US10**, and then click **Generate Code**.

            A code will be shown.
            
            ![Preview on US10](3-preview-2.png)

    - **For EU10:** Just click **SAP Build Apps** and a code will be shown.

        ![Preview on EU10](3-preview-2a.png)

4. Take the code and put enter it in the preview portal in SAP Build Apps.

    Press **Enter**.

    >The pin code can be entered upper or lower case, and you can press **Enter** instead of clicking **Confirm pin**.

    ![Enter code](3-preview-3.png)

    If all goes as planned, you will see that you have been authenticated.

    ![Authenticated](3-preview-3a.png)

    Your preview app will now update and show all the applications in your tenant. You should see something like this:

    ![Preview app authenticated](3-preview-4.PNG)

5. Click the **Shopping App** tile to open the app.

    Click the barcode icon.

    ![Barcode icon](3-preview-5.PNG)

    You can scan this barcode for product **HT-1020**.

    ![HT-1020](barcode.png)

    This should take you to the product details page, where you should see the details for **HT-1020**.

    ![Product details](3-preview-6.PNG)