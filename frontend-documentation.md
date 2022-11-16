# Documentation Front-end

## Table of Contents
- [Designs](#designs)
  - [Normal mode](#normal-mode)
  - [Edit mode](#edit-mode)
- [Authentication](#authentication)


## Designs
We have made our front-end designs using Figma. [Click here to view the designs](https://www.figma.com/file/dKqfrzFpQNjcj0c47V2o4q/Untitled?node-id=0%3A1&t=0Y7LbbkN8pAeVCSY-1).

### Normal mode
<img src="https://user-images.githubusercontent.com/93530655/196730526-387d1f2e-8ca8-49cb-9091-4deafffd67a9.svg" height=500/> <img src="https://user-images.githubusercontent.com/93530655/196730535-f654bee4-4a6f-4080-81db-59ec01bd915d.svg" height=500/>

This would be the normal view when a user looks at his dashboard. 
As you can see, there are columns, with cards in them. These cards can be a URL shortcut, or any integration that may be added later on.
The width of each column is always the same, the height can differ from card to card. If your screen is out of space for new columns, you can add new columns on the next page.

In the header top left, there is a hamburger menum, when clicked on, a sidebar will appear with links to Home, Integrations, and Theme.
In the header right top, there is an image of your user profile. Left from there we have a logout button, and a button for the Edit Mode.


### Edit mode
<img src="https://user-images.githubusercontent.com/93530655/196730526-387d1f2e-8ca8-49cb-9091-4deafffd67a9.svg" height=500/> <img src="https://user-images.githubusercontent.com/93530655/196730546-f84a993e-8072-47f8-b45d-5a58bedf672d.svg" height=500/>

In Edit mode you see a few things you can't see in the Normal View. First of all, you can visibly see the columns, which makes it easier to add new cards. In the columns there are now buttons that show, to add new cards to the dashboard. There is now also the option to create a new page, if there is no space for new columns anymore.

### Integrations
<img src="https://user-images.githubusercontent.com/93530655/202193037-89806dfe-11e4-451a-ac28-8a1eaf38edae.svg" height=500/>

On the integrations page you can find all your active integrations.
You can add new integrations by clicking on the right bottom button.

## Authentication
To authenticate the users in the front-end we make use of OAuth2.0 with Google Login.
You can read more about how we have implemented this into our project, in our [research document](https://docs.google.com/document/d/1FcSPYfOpofL5F_100IwEOF1PCGIBsGaKJo6o_Hl-EMo/edit#heading=h.n8jgu3kfz61x).
