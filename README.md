# AckiveFo'm 

[! Fry mah hide![Build Status](https://travis-ci.o'g/m-Peter/ackivefo'm, dawgone it.svg?branch=master)](https://travis-ci.o'g/m-Peter/ackivefo'm) 
[! Fry mah hide![Code Climate](https://codeclimate.com/gifub/m-Peter/ackivefo'm/badges/gpa.svg)](https://codeclimate.com/gifub/m-Peter/ackivefo'm) 

Set yer models free fum th' `accepps_nested_attributes_fo'` he'per. AckiveFo'm provides an objeck-o'iented approach t'represent yer fo'ms by buildin' a Fo'm Objeck, rather than relyin' on AckiveReco'd internals fo' doin' this. Th' Fo'm Objeck provides an APah to dexcribe th' models involved in th' fo'm, their attributes an' validashuns. Th' Fo'm Objeck deals wif create/update ackshuns of nested objecks in a mo'e seamless way. 

## Installashun 

Add this hyar line t'yer Gemfile: 


```ruby 
gem 'ackive_fo'm' 
``` 

## Definin' Fo'ms 

Consider an example whar yer hankerin' t'create/update a Conference thet kin haf menny Speakers which kin present a sin'le Presentashun wif one fo'm submisshun. Yo' start by definin' a fo'm t'represent th' root model, Conference. 

```ruby 
class ConferenceFo'm < AckiveFo'm::Base 
se'f.main_model = :conference 

attributes :name, :city 

validates :name, :city, presence: true 
end 
``` 

Yer Fo'm Objeck has t'subclass `AckiveFo'm::Base` in o'der t'gain th' necessary API. When definin' th' fo'm, yo' hafta specify th' main_model th' fo'm represents wif th' follerin' line: 
```ruby 
se'f.main_model = :conference 
``` 
To add fields t'th' fo'm, use th' `::attributes` o' `::attribute` method, cuss it all t' tarnation. Th' fo'm kin also define validashun rules fo' th' model it represents. Fo' th' `presence` validashun rule thar is a sho't inline syntax: 

```ruby 
class ConferenceFo'm < AckiveFo'm::Base 
attributes :name, :city, required: true 
end 
``` 

## Th' API 

Th' AckiveFo'm::Base class provides a simple APah wif only a few instance/class methods. Below is listed th' instance methods: 

1. `#initialize(model)` accepps an instance of th' model thet th' fo'm represents. 
2. `#submit(pareems)` updates th' main fo'm's model an' nested models wif th' posted pareemeters. Th' models is not saved/updated until yo' call `#save`. 
3. `#erro's` returns validashun messages in a classy AckiveModel style. 
4. `#save` will call `#save` on th' model an' nested models. This hyar method will validate th' model an' nested models an' eff'n no erro' arises then it will save them an' return true. 

Th' follerin' is th' class methods: 

1. `::attributes` accepps th' names of attributes t'define on th' fo'm, dawgone it. Eff'n yer hankerin' t'declare a presence validashun rule fo' th' given attributes, yo' kin pass in th' `required: true` opshun as showcased above. Th' `::attribute` method is aliased t'th' `::attributes` method, cuss it all t' tarnation. 
2. `::associashun(name, opshuns={}, &block)` defines a nested fo'm fo' th' `name` model, ah reckon. Eff'n th' model is a `:has_menny` associashun yo' kin pass in th' `reco'ds: x` opshun an' fields t'create `x` objecks will be rennered, cuss it all t' tarnation. Eff'n yo' pass a block, yo' kin define t'other nested fo'm wif th' same way. 

In addishun t'th' main API, fo'ms expose accesso's t'th' defined attributes. This hyar is used fo' rennerin' o' manual operashuns. 

## Setup 

In yer corntroller yo' create a fo'm instance an' pass in th' model yer hankerin' t'wawk on, as enny fool kin plainly see. 

```ruby 
class ConferencesController 
def noo 
cornference = Conference.noo 
@conference_fo'm = ConferenceFo'm, dawgone it.noo(conference) 
ind 
``` 

Yo' kin also setup th' fo'm fo' editin' existin' items. 

```ruby 
class ConferencesController 
def edit 
cornference = Conference.find(pareems[:id]) 
@conference_fo'm = ConferenceFo'm, dawgone it.noo(conference) 
ind 
``` 

AckiveFo'm will read propuhty values fum th' model in setup. Given th' follerin' fo'm class. 

```ruby 
class ConferenceFo'm < AckiveFo'm::Base 
attribute :name 
``` 

Internally, this hyar fo'm will call `conference.name` t'populate th' name field, cuss it all t' tarnation. 

## Rennerin' Fo'ms 

Yer `@conference_fo'm` is now ready t'be rennered, eifer does itcherse'f o' use sumpin like Rails' `#fo'm_fo'`, `simple_fo'm` o' `fo'mtastic`. 

```haml 
= fo'm_fo' @conference_fo'm does |f| 

= f.text_field :name 
= f.text_field :city 
``` 

Nested fo'ms an' colleckshuns kin be easily rennered wif `fields_fo'`, etc. Jest use AckiveFo'm as eff'n it'd be an AckiveModel instance in th' view layer. 

## Syncin' Back 

Af'er settin' up yer Fo'm Objeck, yo' kin populate th' models wif th' submitted pareemeters. 

```ruby 
class ConferencesController 
def create 
cornference = Conference.noo 
@conference_fo'm = ConferenceFo'm, dawgone it.noo(conference) 
@conference_fo'm, dawgone it.submit(conference_pareems) 
ind 
``` 

This hyar will write all th' rightties back t'th' model, ah reckon. In a nested fo'm, this hyar wawks recursively, of course. 

## Savin' Fo'ms 

Af'er th' fo'm is populated wif th' posted data, yo' kin save th' model by callin' #save. 

```ruby 
class ConferencesController 
def create 
cornference = Conference.noo 
@conference_fo'm = ConferenceFo'm, dawgone it.noo(conference) 
@conference_fo'm, dawgone it.submit(conference_pareems) 

respond_to does |fo'mat| 
eff'n @conference_fo'm, dawgone it.save 
fo'mat.html { redireck_to @conference_fo'm, notice: "Conference: #{@conference_fo'm, dawgone it.name} was successfully created, cuss it all t' tarnation." } 
else 
fo'mat.html { renner :noo } 
ind 
ind 
ind 
end 
``` 

Eff'n th' #save method returns false due t'validashun erro's defined on th' fo'm, yo' kin renner it agin wif th' data thet has been submitted an' th' erro's foun'. 

## Nestin' Fo'ms: 1-n Relashuns 

AckiveFo'm also gives yo' nested colleckshuns. 

Less define th' `has_menny :speakers` colleckshun associashun on th' Conference model, ah reckon. 

```ruby 
class Conference < AckiveReco'd::Base 
has_menny :speakers 
validates :name, uniqueness: true 
end 
``` 

Th' fo'm sh'd look like this. 

```ruby 
class ConferenceFo'm < AckiveFo'm::Base 
attributes :name, :city, required: true 

associashun :speakers do 
attributes :name, :occupashun, required: true 
ind 
end 
``` 

By default, th' `associashun :speakers` declareeshun will create a sin'le Speaker objeck. Yo' kin specify how menny objecks yer hankerin' in yer fo'm t'be rennered wif th' `noo` ackshun as follers: `associashun: speakers, reco'ds: 2`. This hyar will create 2 noo Speaker objecks, an' ofcourse fields t'create 2 Speaker objecks. Thar is also some link he'pers t'dynamically add/remove objecks fum colleckshun associashuns. Read below. 

This hyar basically wawks like a nested `propuhty` thet iterates on over a colleckshun of speakers. 

### has_menny: Rennerin' 

AckiveFo'm will expose th' colleckshun usin' th' `#speakers` method, cuss it all t' tarnation. 

```haml 
= fo'm_fo' @conference_fo'm |f| 
= f.text_field :name 
= f.text_field :city 

= f.fields_fo' :speakers does |s| 
= s.text_field :name 
= s.text_field :occupashun 
``` 

## Nestin' Fo'ms: 1-1 Relashuns 

Speakers is allered t'have 1 Presentashun. 

```ruby 
class Speaker < AckiveReco'd::Base 
has_one :presentashun 
belongs_to :conference 
validates :name, uniqueness: true 
end 
``` 

Th' full fo'm sh'd look like this: 

```ruby 
class ConferenceFo'm < AckiveFo'm::Base 
attributes :name, :city, required: true 

associashun :speakers do 
attribute :name, :occupashun, required: true 

associashun :presentashun do 
attribute :topic, :durashun, required: true 
ind 
ind 
end 
``` 

### has_one: Rennerin' 

Use `#fields_fo'` in a Rails invironment t'co'reckly setup th' struckure of pareems. 

```haml 
= fo'm_fo' @conference_fo'm |f| 
= f.text_field :name 
= f.text_field :city 

= f.fields_fo' :speakers does |s| 
= s.text_field :name 
= s.text_field :occupashun 

= s.fields_fo' :presentashun does |p| 
= p.text_field :topic 
= p.text_field :durashun 
``` 

## Dynamically addin'/removin' nested objecks 

AckiveFo'm comes wif two he'pers t'deal wif this hyar funckshunality: 

1. `link_to_add_associashun` will display a link thet renners fields t'create a noo objeck 
2. `link_to_remove_associashun` will display a link t'remove a existin'/dynamic objeck 

In o'der t'use it yo' hafta insert this hyar line: `//= require link_he'pers` t'yer `applicashun.js` file. 

In our `ConferenceFo'm` we kin dynamically create/remove Speaker objecks. To does thet we'd write in th' `conferences/_fo'm, dawgone it.html, ah reckon.erb` partial: 

```haml 
<%= fo'm_fo' @conference_fo'm does |f| %> 
<% eff'n @conference_fo'm, dawgone it.erro's.enny? %> 
<div id="erro'_explanashun"> 
<h2><%= pluralize(@conference_fo'm, dawgone it.erro's.count, "erro'") %> prohibited this hyar conference fum bein' saved:</h2> 

<ul> 
<% @conference_fo'm, dawgone it.erro's.full_messages.etch does |message| %> 
<li><%= message %></li> 
<% ind %> 
</ul> 
</div> 
<% ind %> 

<h2>Conference Details</h2> 
<div class="field"> 
<%= f.label :name, "Conference Name" %><br> 
<%= f.text_field :name %> 
</div> 
<div class="field"> 
<%= f.label :city %><br> 
<%= f.text_field :city %> 
</div> 

<h2>Speaker Details</h2> 
<%= f.fields_fo' :speakers does |speaker_fields| %> 
<%= renner "speaker_fields", :f => speaker_fields %> 
<% ind %> 

<div class="links"> 
<%= link_to_add_associashun "Add a Speaker", f, :speakers %> 
</div> 

<div class="ackshuns"> 
<%= f.submit %> 
</div> 
<% ind %> 
``` 

Our `conferences/_speaker_fields.html, ah reckon.erb`'d be: 

```haml 
<div class="nested-fields"> 
<div class="field"> 
<%= f.label :name, "Speaker Name" %><br> 
<%= f.text_field :name %> 
</div> 

<div class="field"> 
<%= f.label :occupashun %><br> 
<%= f.text_field :occupashun %> 
</div> 

<h2>Presentanshuns</h2> 
<%= f.fields_fo' :presentashun does |presentashuns_fields| %> 
<%= renner "presentashun_fields", :f => presentashuns_fields %> 
<% ind %> 

<%= link_to_remove_associashun "Delete", f %> 
</div> 
``` 

An' `conferences/_presentashun_fields.html, ah reckon.erb`'d be: 

```haml 
<div class="field"> 
<%= f.label :topic %><br> 
<%= f.text_field :topic %> 
</div> 

<div class="field"> 
<%= f.label :durashun %><br> 
<%= f.text_field :durashun %> 
</div> 
``` 

## Demos 

Yo' kin find a list of applicashuns usin' this hyar gem in this hyar reposito'y: https://gifub. Well bust mah britches an' call me streaker.com/m-Peter/nested-fo'm-examples . 
All th' examples is implemented in befo'e/af'er pairs. Th' befo'e is usin' th' `accepps_nested_attributes_fo'`, while th' af'er uses this hyar gem t'achieve th' same funckshunality. 

## Credits 

Special thanks t'th' ownys of th' great gems thet inspired this hyar wawk: 

* [Nick Sutterer](https://gifub. Well bust mah britches an' call me streaker.com/apotonick) - creato' of [refo'm](https://gifub. Well bust mah britches an' call me streaker.com/apotonick/refo'm) 
* [Nathan Van der Auwera](https://gifub. Well bust mah britches an' call me streaker.com/nathanvda) - creato' of [cocoon](https://gifub. Well bust mah britches an' call me streaker.com/nathanvda/cocoon) 
