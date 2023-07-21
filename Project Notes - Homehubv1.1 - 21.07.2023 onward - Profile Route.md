#projects 

A fairly large portion of implementing the planned walled garden to allow many users with seperated user groups will be the profile route, which in the current version is completely empty and un-developed. This route will be built in typescript like all the new features for v1.1, so will likely pose a decent amount of difficulty to me as I am learning TS as I go.

## General outline
- Arrange for the auth check post sign-in to route new users straight to their profile page. Greet them with instructions for setting up a user group - **details to be determined**
- Render a component for managing user groups, allowing users to be members of multiple groups if they want. This feature will be actually really useful as it will allow for users to share tasks etc with their housemates, and then also their family for example
	- This component should render a subcomponent for each user group the user is a member of, and an empty component of the same render style with an "add user group" prompt
- Ability to change username?

## Process notes
