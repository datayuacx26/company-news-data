---
schema_version: "1.0.0"
document_id: "2c4f069794dd8d68a2dc30169fbe85fdd2a7cef2027bb8bc72e6e90d328f4ffa"
company_key: "yc-oncactus"
company: "Cactus"
source_id: "yc-oncactus-rss-da464479d2a9"
canonical_url: "https://blog.oncactus.com/p/building-reward-system-rails"
published_at: "2025-01-31T15:59:06+00:00"
first_seen_at: "2026-07-27T00:17:06.713845+00:00"
fetched_at: "2026-07-28T21:59:48.691285+00:00"
content_hash: "sha256:5fe24abec6dfe3329b76a5cdd07e1325d336ab96bfbc858c2c86e17a766dfd6c"
---

# Building a Reward System with Rails

[Engineering](https://blog.oncactus.com/s/engineering/?utm_source=substack&utm_medium=menu)


# Building a Reward System with Rails


[Avinash Joshi](https://substack.com/@itsavinash)


Jan 31, 2025


After


[implementing our basic referral system](https://blog.cravd.com/p/referral-system-in-rails) using the Refer gem, we needed to build a flexible reward system on top of it. This system needed to handle various reward scenarios: when users refer others, when referred users complete trial bookings, and when they subscribe to our service. Here's how we approached this challenge in our Rails monolith application.


## **Understanding the Components**


Before diving into the implementation, let's understand the key components:


-


**Trigger** : The specific event that causes a reward to be created (e.g., completing a trial booking)


-


**Source** : The original cause that led to the reward (typically the referral)


-


**Recipient** : The user or household that receives the reward


-


**Amount** : The reward value, stored in cents for precision using the money-rails gem


## **The Core: The Reward Model**


At the heart of our system is the


` Reward` model:


```text
class Reward < ApplicationRecord
belongs_to :recipient, polymorphic: true
belongs_to :trigger, polymorphic: true, optional: true
belongs_to :source, polymorphic: true, optional: true


monetize :amount_cents


enum reason: {
trial_start: 'trial_start',
sub_complete: 'sub_complete',
signup_discount: 'signup_discount'
}, _prefix: true


scope :unredeemed, -> { where(redeemed_at: nil) }


# ... methods for creating specific rewards ...
end
```


## **The Rewardable Concern**


To make it easy to add reward functionality to different models, we created a Rewardable concern:


```text
# app/models/concerns/rewardable.rb


module Rewardable
extend ActiveSupport::Concern


included do
has_many :rewards, as: :recipient
end


def total_earned
Money.new(rewards.sum(:amount_cents), "CAD")
end


def unredeemed_rewards
rewards.unredeemed
end


def redeem_rewards!
unredeemed_rewards.update_all(redeemed_at: Time.current)
end
end
```


## **Extending the Referral Model**


We extended the Refer gem's Referral model to integrate with our reward system:


```text
module ReferralExtensions
extend ActiveSupport::Concern


included do
has_many :rewards, as: :source
end


def create_trial_rewards(trial_booking)
Reward.create_for_trial_start(self, trial_booking)
end


def complete_subscription(subscription)
Reward.create_for_sub_complete(self, subscription)
end
end


# In config/initializers/refer.rb
Rails.application.config.to_prepare do
Refer::Referral.include ReferralExtensions
end


```


## **Triggering Rewards**


We set up our Booking model to trigger rewards when a booking is completed, as an example:


```text
class Booking < ApplicationRecord
after_save :create_trial_start_reward, if: :status_changed_to_completed?


private


def create_trial_start_reward
referral = Refer::Referral.find_by(referred: household.owner)
Reward.create_for_trial_start(referral, self) if referral
end


def status_changed_to_completed?
saved_change_to_status? && completed?
end
end
```


## **Conclusion**


This flexible reward system allows us to easily add new types of rewards and associate them with different actions in our application. The use of polymorphic associations provides the flexibility to link rewards to various models, while the money-rails gem ensures precise handling of monetary amounts. The system has proven to be robust and easily extendable as our referral program grows.


The combination of the Refer gem for handling referrals and our custom reward system has created a powerful tool for incentivizing user growth and engagement on our platform.
