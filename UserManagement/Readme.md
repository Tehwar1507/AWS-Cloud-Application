**Module 2**: User Authentication and Registration with Amazon Cognito User Pools

In this module you'll make an Amazon Cognito client pool to deal with your clients' records. You'll send pages that empower clients to enroll as another client, check their email address, and sign into the site.

Design Outline At the point when clients visit your site they will initially enlist another client account. For the motivations behind this studio we'll just expect them to give an email address and secret phrase to enroll. Nonetheless, you can design Amazon Cognito to require extra credits in your own applications.

After clients present their enlistment, Amazon Cognito will send an affirmation email with a check code to the location they gave. To affirm their record, clients will get back to your site and enter their email address and the confirmation code they got. You can likewise affirm client accounts utilizing the Amazon Cognito console to utilize counterfeit email addresses for testing.

After clients have an affirmed account (either utilizing the email check process or a manual affirmation through the control center), they will actually want to sign in. At the point when clients sign in, they enter their username (or email) and secret word. A JavaScript capability then speaks with Amazon Cognito, verifies utilizing the Protected Far off Secret word convention (SRP), and gets back a bunch of JSON Web Tokens (JWT). The JWTs contain claims about the character of the client and will be utilized in the following module to confirm against the Relaxing Programming interface you work with Amazon Programming interface Entryway.

![image](https://github.com/user-attachments/assets/f6b18ed4-d013-447b-a089-fa006e39da68)

**Implementation**

Guarantee you've finished the Static Web facilitating step prior to starting the studio.

Every one of the accompanying areas gives an execution outline and point by point, bit by bit directions. The outline ought to give sufficient setting to you to finish the execution in the event that you're now acquainted with the AWS The executives Control center or you need to investigate the administrations yourself without following a walkthrough.

Foundation Amazon Cognito gives two unique components to confirming clients. You can utilize Cognito Client Pools to add join and sign-in usefulness to your application or use Cognito Personality Pools to confirm clients through friendly character suppliers like Facebook, Twitter, or Amazon, with SAML character arrangements, or by utilizing your own character framework. For this module you'll utilize a client pool as the backend for the gave enrollment and sign-in pages.

Utilize the Amazon Cognito control center to make another client pool utilizing the default settings. When your pool is made, note the Pool Id. You'll involve this worth in a later segment.

✅ Bit by bit headings

Go to the Amazon Cognito Control center Pick Deal with your Client Pools. Pick Make a Client Pool Give a name to your client pool, for example, WildRydes, then, at that point, select Survey Defaults

![image](https://github.com/user-attachments/assets/b19fda81-6271-41ba-877d-9be16088a475)


From the Amazon Cognito console select your client pool and afterward select the Application clients segment. Add a new application and ensure the Create client secret choice is deselected. Client insider facts aren't upheld with the JavaScript SDK. On the off chance that you really do make an application with a produced secret, erase it and make another one with the right setup.

✅ Bit by bit headings

From the Pool Subtleties page for your client pool, select Application clients from the Overall settings segment in the left route bar. Pick Add an application client. Give the application client a name like WildRydesWebApp. Uncheck the Create client secret choice. Client mysteries aren't upheld for use with program based applications. Pick Make application client.

![image](https://github.com/user-attachments/assets/421c2213-0594-4ae0-8f61-ad8c83ddeaae)

The /js/config.js file contains settings for the user pool ID, app client ID and Region. Update this file with the settings from the user pool and app you created in the previous steps and commit the file back to your git repository.

✅ Step-by-step directions

On your Cloud9 development environment open js/config.js

Update the cognito section with the correct values for the user pool and app you just created. You can find the value for userPoolId on the Pool details page of the Amazon Cognito console after you select the user pool that you created.

![image](https://github.com/user-attachments/assets/77055f2b-62ee-4a45-b09b-eac785d4e98a)

The incentive for area ought to be the AWS District code where you made your client pool. For example us-east-1 for the N. Virginia District, or us-west-2 for the Oregon Area. In the event that you don't know which code to utilize, you can take a gander at the Pool ARN esteem on the Pool subtleties page. The District code is the piece of the ARN following arn:aws:cognito-idp:.

The refreshed config.js record ought to seem to be this. Note that the genuine qualities for your document will be unique:

```
window._config = {
    cognito: {
        userPoolId: 'us-west-2_uXboG5pAb', // e.g. us-east-2_uXboG5pAb
        userPoolClientId: '25ddkmj4v6hfsfvruhpfi7n4hv', // e.g. 25ddkmj4v6hfsfvruhpfi7n4hv
        region: 'us-west-2' // e.g. us-east-2
    },
    api: {
        invokeUrl: '' // e.g. https://rc7nyt4tql.execute-api.us-west-2.amazonaws.com/prod,
    }
};
```


