---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: baf0f07002a21a8e4e60bc17186107b471243202
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67172429"
---
1. Crie uma nova classe no projeto chamado `ToDoBroadcastReceiver`.
2. Adicionar o seguinte usando as instruções para a classe **ToDoBroadcastReceiver** :

    ```csharp
    using Gcm.Client;
    using Microsoft.WindowsAzure.MobileServices;
    using Newtonsoft.Json.Linq;
    ```

3. Adicione as solicitações de permissão a seguir entre as instruções **using** e a declaração **namespace**:

    ```csharp
    [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
    [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
    [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
    //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
    [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
    [assembly: UsesPermission(Name = "android.permission.INTERNET")]
    [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
    ```

4. Substitua a definição da classe **ToDoBroadcastReceiver** pelo seguinte:

    ```csharp
    [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
    [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, 
        Categories = new string[] { "@PACKAGE_NAME@" })]
    [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, 
        Categories = new string[] { "@PACKAGE_NAME@" })]
    [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, 
    Categories = new string[] { "@PACKAGE_NAME@" })]
    public class ToDoBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
    {
        // Set the Google app ID.
        public static string[] senderIDs = new string[] { "<PROJECT_NUMBER>" };
    }
    ```

    No código acima, você *`<PROJECT_NUMBER>`* deve substituir com o número do projeto atribuído pelo Google quando você provisionou seu aplicativo no portal de desenvolvedores do Google. 

5. No arquivo de projeto ToDoBroadcastReceiver.cs, adicione o código a seguir, que define a classe **PushHandlerService** :

    ```csharp
    // The ServiceAttribute must be applied to the class.
    [Service]
    public class PushHandlerService : GcmServiceBase
    {
        public static string RegistrationID { get; private set; }
        public PushHandlerService() : base(ToDoBroadcastReceiver.senderIDs) { }
    }
    ```

    Observe que essa classe deriva de **GcmServiceBase** e que o atributo **Service** deve ser aplicado a essa classe.

    > [!NOTE]
    > A classe **GcmServiceBase** implementa os métodos **OnRegistered()**, **OnUnRegistered()**, **OnMessage()** e **OnError()**. Você deve substituir esses métodos na classe **PushHandlerService** .

6. Adicione o código a seguir à classe **PushHandlerService** que substitui o manipulador de eventos **OnRegistered**.

    ```csharp
    protected override void OnRegistered(Context context, string registrationId)
    {
        System.Diagnostics.Debug.WriteLine("The device has been registered with GCM.", "Success!");

        // Get the MobileServiceClient from the current activity instance.
        MobileServiceClient client = ToDoActivity.CurrentActivity.CurrentClient;
        var push = client.GetPush();

        // Define a message body for GCM.
        const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

        // Define the template registration as JSON.
        JObject templates = new JObject();
        templates["genericMessage"] = new JObject
        {
            {"body", templateBodyGCM }
        };

        try
        {
            // Make sure we run the registration on the same thread as the activity, 
            // to avoid threading errors.
            ToDoActivity.CurrentActivity.RunOnUiThread(

                // Register the template with Notification Hubs.
                async () => await push.RegisterAsync(registrationId, templates));

            System.Diagnostics.Debug.WriteLine(
                string.Format("Push Installation Id", push.InstallationId.ToString()));
        }
        catch (Exception ex)
        {
            System.Diagnostics.Debug.WriteLine(
                string.Format("Error with Azure push registration: {0}", ex.Message));
        }
    }
    ```

    Esse método usa a ID de registro retornada do GCM para registrar-se no Azure para notificações por push. Marcas só podem ser adicionadas ao registro depois que ele é criado. Para obter mais informações, veja [Como: adicionar marcas à instalação de um dispositivo para habilitar o envio por push para marcas](../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags).

7. Substitua o método **OnMessage** no **PushHandlerService** pelo código a seguir:

    ```csharp
    protected override void OnMessage(Context context, Intent intent)
    {
        string message = string.Empty;

        // Extract the push notification message from the intent.
        if (intent.Extras.ContainsKey("message"))
        {
            message = intent.Extras.Get("message").ToString();
            var title = "New item added:";

            // Create a notification manager to send the notification.
            var notificationManager = 
                GetSystemService(Context.NotificationService) as NotificationManager;

            // Create a new intent to show the notification in the UI. 
            PendingIntent contentIntent =
                PendingIntent.GetActivity(context, 0,
                new Intent(this, typeof(ToDoActivity)), 0);

            // Create the notification using the builder.
            var builder = new Notification.Builder(context);
            builder.SetAutoCancel(true);
            builder.SetContentTitle(title);
            builder.SetContentText(message);
            builder.SetSmallIcon(Resource.Drawable.ic_launcher);
            builder.SetContentIntent(contentIntent);
            var notification = builder.Build();

            // Display the notification in the Notifications Area.
            notificationManager.Notify(1, notification);

        }
    }
    ```

8. Substitua os métodos **OnUnRegistered()** e **OnError()** pelo código a seguir.

    ```csharp
    protected override void OnUnRegistered(Context context, string registrationId)
    {
        throw new NotImplementedException();
    }

    protected override void OnError(Context context, string errorId)
    {
        System.Diagnostics.Debug.WriteLine(
            string.Format("Error occurred in the notification: {0}.", errorId));
    }
    ```
