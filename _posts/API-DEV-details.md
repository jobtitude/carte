## Tools

**Main infrastructure:** Rails Controllers and Roar representers ( [roar](https://github.com/apotonick/roar), [roar-rails](https://github.com/apotonick/roar-rails), [roar samples](http://nicksda.apotomo.de/2012/05/ruby-on-rest-6-pagination-with-roar/) ) for parsing and rendering Json.

We were considering a scenario like with [Grape](https://github.com/intridea/grape#api-formats ) ( coercion, param validation and api standards ), Grape-entity, Grape-swagger, GRape-kaminari, etc... But Sadly Grape has quite donwsides (  Read [this](http://joshsymonds.com/blog/2013/02/22/existing-rails-api-solutions-suck/) ) and doesn't play nice with Devise.

## Rails Routes
- Take care of ```:concerns``` if makes sense
- Keep an eye on Members and Collections ( and [attention](http://guides.rubyonrails.org/routing.html#adding-member-routes) when using ```:on``` or not, it will change the param name )

## Inspiration
- **TeoWaki** Api ( hypermedia, resources, general, validation params ):
https://developers.teowaki.com 
- **Heroku** Api guidance ( naming conventions, ..):
https://github.com/interagent/
- New **Spotify** Web Api ( pagination, pseudo-hypermedia, authorizatio, vesioning ):
https://developer.spotify.com/web-api/ 
- **Github** Api https://developer.github.com/v3/