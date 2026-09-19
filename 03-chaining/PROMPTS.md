# PROMPTS.md: Living Prompt Pack

> Module 3 · Prompt Chaining. Re-architect the build with prompt chains; capture the reusable ones here.

## How to use this pack

_Each prompt is a reusable step. Chain them: the output of one becomes the input to the next._

## Prompt chain: [name your flow]

### Step 1: Expand, build new screens in a strict sequence
```
Build the next phase of this app in a strict sequence:
Keep the exisiting Experiment functionality availible and dont change that, It is a valuable part of the product build and I setill need to run experiments on the product
1. Add a screen "{{User Login}}". Match the layout and spacing of the existing product. There should be two options 1) login as a service Provider 2) Log in as a user of the Marketplace i.e. as someone who is coming to browse the services provided
2. The existing workflow is for users of the Marketplace so map it to when a marketplace user logs in . Lets also provide a place for these users to set up their profile , what services they are interested in and how they would like to pay for their services i,e. Their payment profile. I would alos like to set up a feedback mechanism where in at the end of the booking i would like to understand what component of the service providers profile helped the user make the decision to book with the service provider 
3. Lets create another flow for when a service provider logs in.  in this flow the service provider should be able to provide their metadata , submit verfirication documents, reach out to customer support if needed with any help on their profile, set up the payment profile where the payment goes to them. lets also set up a analytics page for them to show how many users viewed, booked  are thinking about booking services. lets also show these service providers a view of where they are in their verification journey and how much boost they can expect by completeing all of the verification setps required i.e. gaps in their current profile set up that is limiting their reach.  Lets also have a billing page and astatement summary in terms of thier bookings, per booking cycle . Every service provider will be paid out in biweeekly cycles so they can look at their statement for the biweekly period
4. On the Experiment page lets also add a User page in there to show marketplace users who actually viewed and booked services vs who just viewed the services and did not book services. Add analytics that will help showcase how the presence of verification is helping users to transition from a hesitation to confirmed bookings.  Lets also add Service Providerpage to show the difference in service provider metadata for successful bookings vs just views lets also add analytics to showcase if teh verification bades are helping service provioders or not and other anlytics . . lets also have another page where we save event logs across days so that day over day view of the experiment is also captured and saved. 
3. Navigation: from the login page if a user logs in as a Marketplace user then they should be directed to the existing workflow . if they log in as a service provider they should be directed to the service provider space as describer in bullet 3 . there should be navigation path availible to go to the home page from each of the pages 

Maintian the current design layout.
```

### Step 2: Behavior, hard-code the states
```
Apply the following logic constraints to the {{screen}} flow:
- Use a loading bar loading state.
- If no data is present, show the empty state: "Data Not availible"
- On fetch failure, trigger the error state: "404. Page not found". give the users a way to go back to main page 

Maintain the same design language throughout and tether all behavior strictly to these rules.
```

### Step 3: Refine, one surgical polish
```
[write your prompt here, use {{variables}} for the parts you swap]
```

## Reusable techniques learned

- _____
- _____

## What broke (and the fix)

_Where a single mega-prompt failed and chaining fixed it._

_____
