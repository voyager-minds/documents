## Activity flow Diagram [user]

![Alt text](images/user-activity-flow.png 'user activity diagram')

#### code for the diagram

```

title Customer Portal – Search & Reviews

// Pools and Lanes
Customer [color: teal, icon: user] {
  Start [type: event, icon: play]
  Search restaurants [type: activity, icon: search]
  Search food items [type: activity, icon: search]
  View restaurant details [type: activity, icon: map-pin]
  View meal details with price [type: activity, icon: tag]
  View reviews for a restaurant [type: activity, icon: star]
  View reviews for a meal [type: activity, icon: star]
  Write a review [type: activity, icon: edit-3]
  Comment on a review [type: activity, icon: message-circle]
  End [type: event, icon: check]
}

Restaurant Review Portal [color: blue, icon: globe] {
  Moderation {
    Submit post for moderation [type: activity, icon: shield]
    Approve post [type: activity, icon: thumbs-up]
    Reject post [type: activity, icon: thumbs-down]
    Publish post [type: activity, icon: upload]
  }
  Database {
    Fetch meal price [type: activity, icon: database]
  }
}

// Connections (Sequence and Message Flows)
Start > Search restaurants
Start > Search food items

// Search restaurants flow
Search restaurants > View restaurant details
View restaurant details > View reviews for a restaurant
View restaurant details > View meal details with price
View meal details with price --> Fetch meal price : Request price
Fetch meal price --> View meal details with price : Price data
View reviews for a restaurant > Write a review
View reviews for a restaurant > Comment on a review

// Search food items flow
Search food items > View meal details with price
View meal details with price > View reviews for a meal
View reviews for a meal > Write a review
View reviews for a meal > Comment on a review

// Review and comment submission (pre-moderation)
Write a review --> Submit post for moderation : Review content
Comment on a review --> Submit post for moderation : Comment content

// Moderation process
Submit post for moderation > Approve post
Submit post for moderation > Reject post
Approve post > Publish post
Publish post --> Write a review : Review published
Publish post --> Comment on a review : Comment published
Reject post > End

// End events
Write a review > End
Comment on a review > End

// Alternate end from search/browse
View reviews for a restaurant > End
View reviews for a meal > End
View restaurant details > End
View meal details with price > End
Search restaurants > End
Search food items > End


```
