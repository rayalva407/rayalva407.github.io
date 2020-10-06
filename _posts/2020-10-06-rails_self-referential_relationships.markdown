---
layout: post
title:      "Rails Self-Referential Relationships"
date:       2020-10-06 03:26:32 -0400
permalink:  rails_self-referential_relationships
---


In a self-referential relationship a class can interact with itself using a join table. While brainstorming some ideas for my new Rails project I found the topic of self-referential  `has_many, through`relationships. After reading about it I decided to try it out to see if it would fit what I wanted to do. At first it was a little confusing but after some practice with it, I found that it was very simple. I didn't end up using this kind of relationship for my project but it was really fun to learn about it. These sort of relationships are very commonly used in social media. Some examples are followers, friendships, messages, and friend requests. I found that the messages example was the easiest to understand.

## How It Works

Let's create a users table. We want this user to be very simple for now so it will only have a name.


```
#db/migrate/001_create_users.rb

class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.text :name
      t.timestamps
    end
  end
end
```

Next we will create the messages table. This will serve as the join table. through which the users can interact. We will give them a `sender_id` and a `recipient_id` along with the message's content. the `sender_id` and `recipient_id` are both ids that belong to the User table.

```
#db/migrate/001_create_messages.rb

class CreateMessages < ActiveRecord::Migration[6.0]
  def change
     create_table :messages do |t|
        t.text :message
        t.integer :sender_id
        t.integer :recipient_id
        t.timestamps
     end
  end
end
```

Now we have to work with our Message Model. We are going to tell the model that it `belongs_to` two both a Sender and a Recipient. Even though they come from the same class they have different foreign_key names on the messages table.

```
#app/models/message.rb

class Message < ApplicationRecord
  belongs_to :recipient, class_name: "User", foreign_key: "recipient_id"
  belongs_to :sender, class_name: "User", foreign_key: "sender_id"
end
```

We will now set the other end of the relationship in our User model. We are telling the model that it `has_many` received messages and `has_many` sent messages.

```
#app/models/user.rb

class User < ApplicationRecord
  has_many :received_messages, class_name: "Message", foreign_key: "recipient_id"
  has_many :sent_messages, class_name: "Message", foreign_key: "sender_id"
end
```

Let's seed the database and mess around with it.

```
#db/seed.rb

User.create(name: "Ray")
User.create(name: "Joe")
User.create(name: "Bob")

Message.create(message: "Hey", sender_id: User.all.sample.id, recipient_id: User.all.sample.id)
Message.create(message: "Hi", sender_id: User.all.sample.id, recipient_id: User.all.sample.id)
Message.create(message: "Yo", sender_id: User.all.sample.id, recipient_id: User.all.sample.id)
```

On the console.

```
2.6.1 :001 > ray = User.first
   (0.5ms)  SELECT sqlite_version(*)
  User Load (0.2ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT ?  [["LIMIT", 1]]
 => #<User id: 1, name: "Ray", created_at: "2020-10-06 07:15:35", updated_at: "2020-10-06 07:15:35"> 
2.6.1 :002 > joe = User.second
  User Load (0.3ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT ? OFFSET ?  [["LIMIT", 1], ["OFFSET", 1]]
 => #<User id: 2, name: "Joe", created_at: "2020-10-06 07:15:35", updated_at: "2020-10-06 07:15:35"> 
2.6.1 :003 > bob = User.last
  User Load (0.3ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" DESC LIMIT ?  [["LIMIT", 1]]
 => #<User id: 3, name: "Bob", created_at: "2020-10-06 07:15:35", updated_at: "2020-10-06 07:15:35"> 
```
User's received messages.

```
2.6.1 :012 > ray.received_messages.first.message
  Message Load (0.2ms)  SELECT "messages".* FROM "messages" WHERE "messages"."recipient_id" = ? ORDER BY "messages"."id" ASC LIMIT ?  [["recipient_id", 1], ["LIMIT", 1]]
 => "Hey" 
2.6.1 :013 > bob.received_messages.first.message
  Message Load (0.2ms)  SELECT "messages".* FROM "messages" WHERE "messages"."recipient_id" = ? ORDER BY "messages"."id" ASC LIMIT ?  [["recipient_id", 3], ["LIMIT", 1]]
 => "Yo" 
2.6.1 :014 > joe.received_messages.first.message
  Message Load (0.2ms)  SELECT "messages".* FROM "messages" WHERE "messages"."recipient_id" = ? ORDER BY "messages"."id" ASC LIMIT ?  [["recipient_id", 2], ["LIMIT", 1]]
 => "Hi" 
```

User's sent messages.

```
2.6.1 :020 > bob.sent_messages.first.message
  Message Load (0.2ms)  SELECT "messages".* FROM "messages" WHERE "messages"."sender_id" = ? ORDER BY "messages"."id" ASC LIMIT ?  [["sender_id", 3], ["LIMIT", 1]]
 => "Hi" 
2.6.1 :021 > joe.sent_messages.first.message
  Message Load (0.2ms)  SELECT "messages".* FROM "messages" WHERE "messages"."sender_id" = ? ORDER BY "messages"."id" ASC LIMIT ?  [["sender_id", 2], ["LIMIT", 1]]
 => "Hey" 
2.6.1 :022 > 
```

That is all! Make sure to mess with different kinds of models so that it is a little bit easier to understand. I am sure there are many ways to have fun with this relationship. Happy coding!
