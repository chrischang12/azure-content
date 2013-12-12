<properties linkid="develop-mobile-tutorials-authorize-users-in-scripts-android" urlDisplayName="Authorize Users in Scripts (Android)" pageTitle="Authorize users in scripts (Android) | Mobile Dev Center" metaKeywords="Windows Azure authorizing user, android scripts authorization, authorize mobile services, Android azure security" description="Learn how to authorize users in scripts in your Android app with Windows Azure Mobile Services." metaCanonical="" services="" documentationCenter="Mobile" title="Use scripts to authorize users in Mobile Services" authors=""  solutions="" writer="" manager="" editor=""  />



# Use scripts to authorize users in Mobile Services
<div class="dev-center-tutorial-selector sublanding"> 
	<a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-dotnet" title="Windows Store C#">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-js" title="Windows Store JavaScript">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-ios" title="iOS">iOS</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-android" title="Android" class="current">Android</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-html" title="HTML">HTML</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-xamarin-ios" title="Xamarin.iOS">Xamarin.iOS</a><a href="/en-us/develop/mobile/tutorials/authorize-users-in-scripts-xamarin-android" title="Xamarin.Android">Xamarin.Android</a>
</div>	

This topic shows you how to use server scripts to authorize authenticated users for accessing data in Windows Azure Mobile Services from an Android app.  In this tutorial you register scripts with Mobile Services to filter queries based on the userId of an authenticated user, ensuring that each user can see only their own data.

This tutorial is based on the Mobile Services quickstart and builds on the previous tutorial [Get started with authentication]. Before you start this tutorial, you must first complete [Get started with authentication].  

## <a name="register-scripts"></a>Register scripts
Because the quickstart app reads and inserts data, you need to register scripts for these operations against the TodoItem table.

1. Log on to the [Windows Azure Management Portal], click **Mobile Services**, and then click your app. 

   ![][0]

2. Click the **Data** tab, then click the **TodoItem** table.

   ![][1]

3. Click **Script**, then select the **Insert** operation.

   ![][2]

4. Replace the existing script with the following function, and then click **Save**.

        function insert(item, user, request) {
          item.userId = user.userId;    
          request.execute();
        }

    This script adds a userId value to the item, which is the user ID of the authenticated user, before it is inserted into the TodoItem table. 

    <div class="dev-callout"><b>Note</b>
	<p>Dynamic schema must be enabled the first time that this insert script runs. With dynamic schema enabled, Mobile Services automatically adds the <strong>userId</strong> column to the <strong>TodoItem</strong> table on the first execution. Dynamic schema is enabled by default for a new mobile service, and it should be disabled before the app is published to the Windows Store.</p>
    </div>


5. Repeat steps 3 and 4 to replace the existing **Read** operation with the following function:

        function read(query, user, request) {
           query.where({ userId: user.userId });    
           request.execute();
        }

   This script filters the returned TodoItem objects so that each user only receives the items that they inserted.

## Test the app

1. In Eclipse, open the project that you modified when you completed the tutorial [Get started with authentication].

2. From the **Run** menu, then click **Run** to start the app and sign in with your chosen identity provider. 

   Notice that this time, although there are items already in the TodoItem table from previous tutorials, no items are returned. This happens because previous items were inserted without the userId column and now have null values.

3. In the app, enter text in **Insert a TodoItem** and then click **Save**.

   This inserts both the text and the userId in the TodoItem table in the mobile service. Because the new item has the correct userId value, it is returned by the mobile service and displayed in the second column.

5. Back in the **todoitem** table in the [Management Portal][Windows Azure Management Portal], click **Browse** and verify that each newly added item now has an associated userId value.

6. (Optional) If you have additional login accounts, you can verify that users can only see their own data by closing the app and then running it again. When the login credentials dialog is displayed, enter a different login, and then verify that the items entered under the previous account are not displayed.

## Next steps

This concludes the tutorials that demonstrate the basics of working with authentication. Consider finding out more about the following Mobile Services topics:

* [Get started with data]
  <br/>Learn more about storing and querying data using Mobile Services.

* [Get started with push notifications] 
  <br/>Learn how to send a very basic push notification to your app.

* [Mobile Services server script reference]
  <br/>Learn more about registering and using server scripts.

<!-- Anchors. -->
[Register server scripts]: #register-scripts
[Next Steps]:#next-steps

<!-- Images. -->
[0]: ./media/mobile-services-android-authorize-users-in-scripts/mobile-services-selection.png
[1]: ./media/mobile-services-android-authorize-users-in-scripts/mobile-portal-data-tables.png
[2]: ./media/mobile-services-android-authorize-users-in-scripts/mobile-insert-script-users.png


<!-- URLs. -->
[Mobile Services server script reference]: http://go.microsoft.com/fwlink/p/?LinkId=262293
[My Apps dashboard]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Get started with Mobile Services]: /en-us/develop/mobile/tutorials/get-started-android
[Get started with data]: /en-us/develop/mobile/tutorials/get-started-with-data-android
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-android
[Get started with push notifications]: /en-us/develop/mobile/tutorials/get-started-with-push-android

[Windows Azure Management Portal]: https://manage.windowsazure.com/