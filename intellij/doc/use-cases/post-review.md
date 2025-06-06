# Post Review

## 1. Primary actor and goals
__Primary Actor__:

* __User__(Registered): Intends to post a review for a book they have read.
* Wants an intuitive and fast review posting experience with real-time updates after submission.

__Goals__:
* Allow users to seamlessly post reviews for books within the app while ensuring the input is processed, validated, and displayed instantly.

## 2. Preconditions
* User must be registered and authenticated (logged in).
* The book for which the review is being posted exists in the app's database. 
* The app has an active internet connection to post the review to the server backend.


## 3. Postconditions

The system ensures the following upon successful completion of this use case:
* The review is saved in the database against the book and user profile. 
* The review becomes visible to other users in real-time. 
* Any associated rating with the review (e.g., a star rating) 
updates the book's average rating. 
* Optionally, trigger notifications to friends/followers of this activity.

__Failure Scenarios__:
* If the review cannot be saved due to network or server issues, 
the app must notify the user and prompt the to retry.


## 4. Workflow
__Fully-dressed workflow diagram__:


```plantuml
@startuml

skin rose

title Post Review - Fully Dressed

|#application|User|
|#technology|System|


|User|
start
:Log in to the app;
:Navigate to the book details page;
:Click on "Add Review" button;
repeat
|System|
:Display review input form;

|User|
:Fill in review details including star rating;
:Submit the review;

|System|

backward: Show error message: Unsuccessful submission;
repeat while (Submission successful?) is (no)
-> yes;
  :Process and save review;
  :Update book's overall rating;
  :Show "Review posted successfully" confirmation;

|User|
:View posted review in the book's review feed;
stop

@enduml
```

---
## Sequence Diagram

```plantuml
@startuml
skin rose

actor User
participant ViewBookFragment as VBF
participant PostReviewDialogFragment as PRDF
participant Review
participant ReviewsAdapter as ADA


User -> VBF: click postReviewButton
activate VBF
VBF -> PRDF: new PostReviewDialogFragment()
VBF -> PRDF: setOnReviewSubmittedListener(callback)
VBF -> PRDF: show()
deactivate VBF

User -> PRDF: set rating and comment
User -> PRDF: click submitButton
activate PRDF
PRDF -> VBF: onReviewSubmitted(rating, comment)
PRDF -> PRDF: dialog.dismiss()
deactivate PRDF

activate VBF
VBF -> Review: «create» Review(username, rating, comment)
VBF -> VBF: reviews.add(newReview)
VBF -> ADA: notifyItemInserted(index)
deactivate VBF
@enduml
```
