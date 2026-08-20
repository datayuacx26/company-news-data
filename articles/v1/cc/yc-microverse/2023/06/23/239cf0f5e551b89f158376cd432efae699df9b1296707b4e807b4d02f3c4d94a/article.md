---
schema_version: "1.0.0"
document_id: "239cf0f5e551b89f158376cd432efae699df9b1296707b4e807b4d02f3c4d94a"
company_key: "yc-microverse"
company: "Microverse"
source_id: "yc-microverse-news-import-bead39b6c183"
canonical_url: "https://www.microverse.org/blog/understanding-rspec-model-and-helper-specs"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-07-22T04:19:00.975368+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:fc800e6c9bb67b886325c32782a9786761940d1bbbaf33dc213dbda3e5db5aba"
---

# Beginners Guide to Understand RSpec Model and Helper Specs

As a developer, it's common practice to break down complex problems into smaller parts. When we test each part individually, it's called **unit testing.** This testing is typically done on the smallest parts of the application. After reading this article, you'll feel confident working with Model and Helper Specs.


## **Working With Rails Model Specs**


Model specs are the most important test you can run in your application. They are easy to work with, mainly because, unlike controllers, there is no request/response cycle to worry about - there is nothing like rendering a template.


As a beginner, model specs are the easiest to take on and add a lot of value. They are fairly straightforward, but I'm going to share a few pointers that will help you get past the most common stumbling blocks:


- There may be times you need to #reload after making changes in the database to ensure you're working with a fresh copy of the database. This won't happen all the time, so you certainly shouldn't overuse it.
- Understand the difference between the lazy-executing #let versus eagerly-executing #let!. The first doesn't get executed until it's called, while #let! forces it to happen right away.
- Another thing people often need clarification on is how to write tests for Scopes. Scopes:match_array works with ActiveRecord::Relations.


{% code-block language="js" %}
describe 'Users' do
let!(:users) do
\[User.create, User.create, User.create\]
end
it 'uses match_array to match a scope' do
expect(User.all).to match_array(users)
end
end
{% code-block-end %}


## **How to Test Associations & Validations**


Writing complex code will allow you to poke around in Rails internals to determine your test expectations. An easier way to do this, though, is to use a popular third-party library by *thoughtbot* called[shoulda-matchers](https://matchers.shoulda.io/docs/v4.3.0/) . This library gives you a lot of useful matchers to play with. For example; have_many, validate_presence_of.


Did you notice a slight difference there? It says “ **have_many** ” not “ **has_many** ” because it’s a *matcher* meant to be used with an *expectation* . So we would expect users to “ **have_many** ” posts. In the definition we say, has_many :posts but in our expectations, we say — we expect users to have_many orders. The same applies to validation.


Configuring **shoulda-matchers** is relatively easy as they have very good[documentation](https://matchers.shoulda.io/docs/v4.3.0/) . To start, add the[shoulda-matcher gem](https://rubygems.org/gems/shoulda-matchers) to your gem file.


{% code-block language="js" %}
group :test do
gem 'rails-controller-testing' # If you are using Rails 5.x
gem 'rspec'
gem 'shoulda-matchers', '4.0.0.rc1'
end
{% code-block-end %}


Place the code below at the bottom of spec/rails_helper.rb


{% code-block language="js" %}
Shoulda::Matchers.configure do |config|
config.integrate do |with|
with.test_framework :rspec
with.library :rails
end
end
{% code-block-end %}


I encourage you to take a look at the[shoulda-matchers documentation](https://matchers.shoulda.io/docs/v4.3.0/) , there are great examples there that will guide you in writing yours.


The examples below will give you better insights on using shoulda-matchers for both your validations and association testing.


#### **Validations**


{% code-block language="js" %}
validates_uniqueness_of :username, case_sensitive: false, message: ‘Username already taken.’
validates_presence_of :username, message: ‘Username cannot be blank’
validates_presence_of :fullname, message: ‘FullName cannot be blank’
validates :username, length: { minimum: 3, maximum: 10,
too_long: ‘Maximum allowed username is 10 characters.’,
too_short: ‘Minimum allowed characters for username is 3’ }
validates :fullname, length: { minimum: 6, maximum: 20,
too_long: ‘Maximum allowed fullname is 20 characters.’,
too_short: ‘Minimum allowed characters for fullname is 6’ }
{% code-block-end %}


##### Tests for Validations Above


{% code-block language="js" %}
describe ‘Validations’ do
it do
should validate_presence_of(:username).with_message(‘Username cannot be blank’)
end
it do
should validate_length_of(:username).is_at_most(10)
.with_message(‘Maximum allowed username is 10 characters.’)
end
it { should_not validate_length_of(:username).is_at_least(2) }
it { should validate_uniqueness_of(:username).case_insensitive.with_message(‘Username already taken.’) }
end
{% code-block-end %}


#### **Association Tests**


{% code-block language="js" %}
describe ‘Associations’ do
it { should have_many(:opinions).with_foreign_key(:author_id) }
it { should have_many(:follows).through(:followings) }
end
{% code-block-end %}


## **Working with ActiveRecord Test Doubles**


Another useful tool for working with ActiveRecord test doubles is[rspec-activemodel-mocks.](https://rubygems.org/gems/rspec-activemodel-mocks) This tool gives us two additional methods to use: *mock-model* and *stub-model* .


The mock-model is used to create a double of an ActiveRecord object. What's unique about it is that it already has stubbed most of the methods that are included in the ActiveRecord object, like *save* , *create* , and *build,* for you.


You can use 'stub-model' to stub certain behaviors. ActiveRecord test doubles become most useful when creating something that acts like an ActiveRecord object but doesn't hit the database. You can check out more in the[documentation](https://www.rubydoc.info/gems/rspec-activemodel-mocks/1.1.0) .


Using these tips and techniques, you should be able to write specs for all of your Rails app models.


## **Working With Rails Helper Specs**


Working with Rails helper specs is almost as easy as working with models. We can work with them in isolation, as there is no request/response cycle for us to worry about.


However, there is one important feature to note, and an object called *helper* that can be used in our examples.


Let's start by creating a spec helper file:


{% code-block language="js" %}
$ rails g rspec:helper application
{% code-block-end %}


We will define a simple method inside our helpers/application_helper.rb file for demonstration.


{% code-block language="js" %}
module ApplicationHelper
def interest_val(capital)
capital * 0.05
end
end
----------------------------------------------------------
# inside our spec/helpers/application_helper_spec.rb file
...
RSpec.describe ApplicationHelper, type: :helper do
describe "#interest_val" do
it "returns the interest on a capital" do
expect(helper.interest_val(1000)).to eq(50)
end
end
end
{% code-block-end %}


In the above example, you’ll notice how we called the method using the helper. Remember helpers are ruby modules, not ruby classes. We could have simply included the module, then had access to all the methods in our spec files.


However, using the helper has two advantages;


1. The helper method also includes all of the rails built-in helpers, which automatically gives us access to any of those
2. We can assign an instance variable that will normally be present when any of our helper methods are used inside the view. The example below will give you more insight into what I mean here.


{% code-block language="js" %}
module ApplicationHelper
def next_page
(@page || 0) + 1
end
end
----------------------------------------------------------
# inside our spec/helpers/application_helper_spec.rb file
...
RSpec.describe ApplicationHelper, type: :helper do
describe "#next_page" do
it "returns @page plus 1" do
assign(:page, 5)
expect(helper.next_page).to eq(6)
end
end
end
{% code-block-end %}


If #next_page method doesn't recognize it, it will initialize 0 and add 1 to it. Accessing the method via the helper object is like using the technique in its proper context, instead of trying to use it abstractly out of context.


For more examples of this, see the[helper spec docs.](https://relishapp.com/rspec/rspec-rails/v/4-0/docs/helper-specs/helper-spec)


After going through this guide, you should be more confident in conducting tests on your model and helper specs. Model specs are crucial in unit testing - a vital aspect of Test Driven Development. Unit testing guarantees that the tiniest components of your application are functioning correctly and should not be skipped. Happy coding!


***To learn more about Microverse, and our supportive community of remote software developers,***[apply to join today](https://apply.microverse.org/?utm_source=blog%20footer&utm_medium=banner&utm_campaign=newblogbar) ***!***
