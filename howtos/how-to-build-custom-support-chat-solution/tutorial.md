# How to build a custom support chat solution with TalkJS

TalkJS is a highly customizable chat API that allows developers to build chat experiences quickly, in a matter of hours instead of weeks. TalkJS comes with a highly configurable, pre-built UI which can be styled and tweaked to meet various use cases. In this tutorial we will build a custom livechat solution with TalkJS.


## Introduction

TalkJS can be used to build a custom chat solution for communicating with your clients. In fact, we use such a tool here at TalkJS.

## Things to Do
- Setting up our chat
- Adding features
- Styling the chat

## Setting up our chat

To set up our chat we will follow the [Getting Started](https://talkjs.com/docs/Getting_Started/) instructions in the Docs. This gives us a working inbox which we can then customize. Remember to use your TalkJS appId, this is found in the [dashboard](https://talkjs.com/dashboard).

### Overview
If you are new to TalkJS this section quickly goes over the basic steps we've done. 

First we embed the script for the TalkJS API in our app. This loads the TalkJS API from the CDN. We also specify a div where the chatbox will be rendered once it's fetched from the CDN.

We then use another script to initiate our chat. This involves defining our users. This is also where we need the appId. One of our users, Alice, is made a current user. This could be the admin or service provider.

We create another user, to whom Alice will talk to, and then create a conversation between the two using a unique ID. Once the conversation is created, we set the participants and then create the chatbox for that conversation. After the chatbox has been created, we mount it inside the div with the id ‘talkjs-container’.

In this scenario, Sebastian is the person to whom we are talking to. In production, he would be client who wants to chat with the service provider.

Lastly we create a variable to hold our chatbox and then mount it.

After following these steps our chatbox should look as below:

![initial](./images/initial.png)



## Adding Features

To give our customer support a more human feel we want to add a profile picture to our user. To do this we include a `photoUrl` property in the User Object, which is a link to the profile picture. If nothing is specified here, by default it displays the initials of the user.

We can edit our code to this:

```
var me = new Talk.User({
    id: "103489",
    name: "Alice",
    email: "alice@example.com",
    photoUrl: "images/alice.png"
});

```
We also want to add a default message that a customer sees when they open the chat, for example containing a link to docs, availability or other resources. ( Such as you see on the TalkJS livechat). 

To do this we use the `setAttributes` method. [`Session.setAttributes`]() can also be used to set a subject, and even a display picture of the whole chat, as opposed to the profile pictures on individual participants.

So we can add the following code:

```
conversation.setAttributes({
    subject: "Global Webhosts",
    photoUrl: "images/logo.png",
    welcomeMessages: [
        "Hi, welcome. We are usually online during working hours but feel free to leave a message at any time.",
        "You can also check our <http://example.com|docs>" 
    ]
})

```

Our chatbox now looks like this:

![intermediate](./images/inter.png) 

By default our chat is an inbox UI panel. However for most customer support livechats, it is preferreble to have a popup that can be minimized.

To implement the popup interface we use `Session.createPopup` to create it, and then call it using `mount`.

In our code we can replace:

```
const inbox = talkSession.createInbox({ selected: conversation });
inbox.mount(document.getElementById('talkjs-container'));
```
with: 

```
const chatPopup = talkSession.createPopup(conversation, {
    keepOpen: false
});
chatPopup.mount({ show: true }); 

```

Our chat will look as shown below:

![chatPopup](images/popup.png)

## Styling
TalkJS comes with a very flexible and highly configurable UI and it is up to the developer to tweak around in the Theme Editor to get a particular look. The Theme Editor allows edits using CSS. By targeting relevant  CSS classes, it is fast and easy to make edits in our layouts.

In this tutorial we will only apply minimal styling just to show what is possible.

In our TalkJS dashboard we navigate to Theme Editor and to the ChatHeader Component. 

### The `ChatHeader` Component

The ChatHeader component contains the UI code for the chatbox or inbox header. 

To remove the user lists we need to delete line 45:

```
<div class="subtitle">{{ names }}</div>

```
We also want to get rid of the search box, so that our chat looks like the TalkJS one.

In line 52 we can delete the following code, which removes the search icon:

```
<SearchButton t:if="{{ showSearchButton }}" class="action-button">
    <Icon type="search" />
</SearchButton>
```
Our chat pop up will look like this:

![final](./images/final.png)

We have only scratched the surface of what is possible using the Theme Editor, many more possibilities exist. For example, one can style it to look like [WhatsApp](https://dev.to/talkjs/building-a-whatsapp-clone-with-talkjs-chat-api-cdf), or [Discord](https://dev.to/talkjs/how-to-create-a-chat-like-discord-with-talkjs-4660), or [Slack](https://dev.to/talkjs/how-to-create-a-slack-like-chatbox-with-talkjs-themes-34bf).


## Conclusion

TalkJS is a powerful tool that can reduce development time while at the same time offering the flexibility to customize it as you like using the Theme Editor.


