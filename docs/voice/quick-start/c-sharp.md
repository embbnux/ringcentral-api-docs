no_breadcrumb:true

# RingOut C# Quick Start

Welcome to the RingCentral Platform. RingCentral is the leading unified communications platform. From one system developers can integrate with, or build products around all the ways people communicate today: SMS, voice, fax, chat and meetings.

In this Quick Start, we are going to help you connect two people in a live phone call using our RingOut API, which dials two phone numbers, and then connects the two people when they answer. Let's get started.

## Create an App

The first thing we need to do is create an app in the RingCentral Developer Portal. This can be done quickly by clicking the "Create RingOut App" button below. Just click the button, enter a name and description if you choose, and click the "Create" button. If you do not yet have a RingCentral account, you will be prompted to create one.

<a target="_new" href="https://developer.ringcentral.com/new-app?name=RingOut+Quick+Start+App&desc=A+simple+app+to+demo+placing+a+call+on+RingCentral&public=false&type=ServerOther&carriers=7710,7310,3420&permissions=RingOut&redirectUri=" class="btn btn-primary">Create RingOut App</a>
<a class="btn-link btn-collapse" data-toggle="collapse" href="#create-app-instructions" role="button" aria-expanded="false" aria-controls="create-app-instructions">Show detailed instructions</a>

<div class="collapse" id="create-app-instructions">
<ol>
<li><a href="https://developer.ringcentral.com/login.html#/">Login or create an account</a> if you have not done so already.</li>
<li>Go to Console/Apps and click 'Create App' button.</li>
<li>Give your app a name and description, then click Next.</li>
<li>On the second page of the create app wizard enter the following:
  <ul>
  <li>Select 'Private' for Application Type.</li>
  <li>Select 'Server-only (No UI)' for Platform Type.</li>
  </ul>
  </li>
<li>On the third page of the create app wizard, select the following permissions:
  <ul>
    <li>RingOut</li>
  </ul>
</li>
<li>We are using Password Flow authentication, so leave "OAuth Redirect URI" blank.</li>
</ol>
</div>

When you are done, you will be taken to the app's dashboard. Make note of the Client ID and Client Secret. We will be using those momentarily.

## Place a Call

### Create a Visual Studio project

* Choose Console Application .Net Core -> App
* Select Target Framework .NET Core 2.1
* Enter project name "Call_Ringout"
* Add NuGet package RingCentral.Net (1.2.1) SDK

### Edit the file Program.cs

Be sure to edit the variables in ALL CAPS with your app and user credentials. Be sure to also set the recipient's phone number.

``` c#
using System;
using System.Threading.Tasks;
using RingCentral;

namespace Call_Ringout
{
    class Program
    {
          const string RECIPIENT = "<ENTER PHONE NUMBER>";

          const string RINGCENTRAL_CLIENTID = "<ENTER CLIENT ID>";
          const string RINGCENTRAL_CLIENTSECRET = "<ENTER CLIENT SECRET>";

          const string RINGCENTRAL_USERNAME = "<YOUR ACCOUNT PHONE NUMBER>";
          const string RINGCENTRAL_PASSWORD = "<YOUR ACCOUNT PASSWORD>";
          const string RINGCENTRAL_EXTENSION = "<YOUR EXTENSION, PROBABLY '101'>";

          static RestClient restClient;

          static void Main(string[] args)
          {
              restClient = new RestClient(RINGCENTRAL_CLIENTID, RINGCENTRAL_CLIENTSECRET, RINGCENTRAL_PRODUCTION);
              restClient.Authorize(RINGCENTRAL_USERNAME, RINGCENTRAL_EXTENSION, RINGCENTRAL_PASSWORD).Wait();
              call_ringout().Wait();
          }
          static private async Task call_ringout()
          {
              var parameters = new MakeRingOutRequest();
              parameters.from = new MakeRingOutCallerInfoRequestFrom { phoneNumber = RINGCENTRAL_USERNAME };
              parameters.to = new MakeRingOutCallerInfoRequestTo {  phoneNumber = RECIPIENT } ;
              parameters.playPrompt = false;

              var resp = await restClient.Restapi().Account().Extension().RingOut().Post(parameters);
              Console.WriteLine("Call Placed. Call status" + resp.status.callStatus);
          }
      }
}
```

### Run Your App

You are almost done. Now run your app from Visual Studio.

## Need Help?

Having difficulty? Feeling frustrated? Receiving an error you don't understand? Our community is here to help and may already have found an answer. Search our community forums, and if you don't find an answer please ask!

<a target="_new" href="https://forums.developers.ringcentral.com/search.html?c=11&includeChildren=false&f=&type=question+OR+kbentry+OR+answer+OR+topic&redirect=search%2Fsearch&sort=relevance&q=voice">Search the forums &raquo;</a>

## What's Next?

When you have successfully made your first API call, it is time to take your next step towards building a more robust RingCentral application. 

<a class="btn btn-success btn-lg" href="../../../basics/your-first-steps/">Take your next step &raquo;</a>

