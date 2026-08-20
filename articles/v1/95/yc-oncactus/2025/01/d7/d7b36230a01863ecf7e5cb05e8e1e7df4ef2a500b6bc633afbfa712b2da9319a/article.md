---
schema_version: "1.0.0"
document_id: "d7b36230a01863ecf7e5cb05e8e1e7df4ef2a500b6bc633afbfa712b2da9319a"
company_key: "yc-oncactus"
company: "Cactus"
source_id: "yc-oncactus-rss-da464479d2a9"
canonical_url: "https://blog.oncactus.com/p/referral-system-in-rails"
published_at: "2025-01-12T23:34:15+00:00"
first_seen_at: "2026-07-27T00:17:06.713845+00:00"
fetched_at: "2026-07-28T22:01:05.353137+00:00"
content_hash: "sha256:481c20cccc5018bb7839899805c67da201b7a306371dab7042a246e4139decab"
---

# Implementing a Referral System in Rails

[Engineering](https://blog.oncactus.com/s/engineering/?utm_source=substack&utm_medium=menu)


# Implementing a Referral System in Rails


[Avinash Joshi](https://substack.com/@itsavinash)


Jan 12, 2025


We recently implemented a referral system for Cravd, our marketplace platform that connects customers with personal home chefs. As we've been growing organically through word of mouth, we wanted to accelerate this growth by implementing a formal referral program. Here's how we set it up using the Refer gem in our Rails application.


## **Setting Up the Refer Gem**


First, we added the


[Refer gem](https://github.com/excid3/refer) to our Gemfile:


```text
gem "refer"
```


After running


` bundle install` , we generated the necessary migrations:


```text
rails generate refer:install
rails db:migrate
```


This set up the basic database structure for tracking referrals.


## **Configuring the Referral Code Generator**


We customized the referral code generation in an initializer file:


```text
# config/initializers/refer.rb


Refer.code_generator = lambda do |referrer|
get_existing_code(referrer).presence || generate_new_code(referrer.id)
end


Refer.param_name = :code


private


def get_existing_code(referrer)
referrer.referral_codes.last&.code
end


def generate_new_code(referrer_id)
letters = ("A".."Z").to_a.sample(4).join
padded_id = referrer_id.to_s.rjust(4, "0")
"#{letters}-#{padded_id}"
end
```


This creates unique, readable codes combining random letters and the referrer's ID.


## **Integrating with User Model**


We added the


` has_referrals` macro to our User model:


```text
class User < ApplicationRecord
has_referrals
# ... other code ...
end
```


## **Handling Referrals in the Controller**


We updated our RegistrationsController to handle referrals during sign up:


```text
class RegistrationsController < ApplicationController
def create
@user = User.new(user_params)
if @user.save
refer @user
start_new_session_for @user
send_welcome_email
redirect_to after_authentication_url, notice: "Welcome to Cravd!"
else
render :new, status: :unprocessable_content
end
end
end
```


## **Handling Referrals in the Controller**


In our


` ReferralsController` , we implemented two key methods to manage referral codes and cookies:


```text
class ReferralsController < ApplicationController
set_referral_cookie only: [:show], if: -> { Current.user.blank? && !browser.bot? }
before_action :set_referral_code, only: [:index, :show]


private


def set_referral_code
return unless Current.user
@referral_code = Current.user.referral_codes.last || Current.user.referral_codes.create
end
end
```


The


` set_referral_cookie` method is used to store the referral code in a cookie when it's present in the request parameters, and


` set_referral_code` method is used to create or retrieve a referral code for the current user. It’s used in the referrals controllers where I want to display the user's referral code.


## **Conclusion**


With these pieces in place,


[we now have a functional referral system](https://cravd.com/refer) . Users can generate unique referral codes, share them, and we can track successful referrals. This implementation provides a solid foundation for our referral program, which we'll build upon to create a comprehensive reward system.
