# BDD for CakePHP applications :: CakeFest2012

Slide: http://twl.sh/NFakdY


## 1. About BDD

Behavior-Driven Development (abbreviated BDD) is a software development process based on test-driven development (TDD).

### Why now, needs BDD?

Why to make application?
For myself? For my interesting? For self-complacency?
So, it is ok if the application is hobby or inside only your-self.
In the business is "No!"

Because, to make application for our clients happy.
How make clients happy?
Client has interest about the business. Maybe the interest does not include technical or awesome frameworks.

I think that you need communication for client happiness.
BDD frameworks help to communicate between client and developer.
And BDD match agile development.

Behavior-driven development borrows the concept of the ubiquitous language from domain driven design.
A ubiquitous language is a formal language that is shared by all members of a software development team (with client).
The language in question is both used and developed by all team members as a common means of discussing the domain of the software in question.
In this way BDD becomes a vehicle for communication between all the different roles in a software project.

### BDD frameworks

BDD has 2 frameworks.
Spec and Story.

Story framework help to communicate between client and developer.
Because story framework use user stories as an input format for test scenarios.

Spec framework don't use user stories as an input format for test scenarios but rather use functional specifications for units that are being tested. 
These specification are more technically than user stories and are usually less convenient for communication with client than are user stories.
Spec framework is for developer, and it often have been used as document.

## 2. Spec frameworks

We can find some spec frameworks on github repositories.

* PHPSpec
  PHPSpec is major framework for BDD of PHP.
* pecs
  it was inspired on JSpec. But it not maintain at now.
* spectrum
  This is alpha version and document is only russian.
* speciphy
  it is inspired by RSpec2 based on PHPSpec. Writing format used closure.
* Spec for PHP
  it was inspired on RSpec. It awesome framework, because I think good that be able to use natural language syntax to express expectations used by DSL. It is easy and reader-friendly !!
  
### PHPSpec example 

<pre><code>
class DescribeNewBowlingGame extends \PHPSpec\Context {  
    private $_bowling = null;  
    public function before() {  
        $this->_bowling = $this->spec(new Bowling);  
    }  
    public function itShouldScore0ForGutterGame() {  
        for ($i=1; $i<=20; $i++) {  
            // someone is really bad at bowling!  
            $this->_bowling->hit(0);  
        }  
        $this->_bowling->score->should->equal(0);  
    }  
}  
</code></pre>

Style of PHPSpec is like a PHPUnit. At first extends Context class, and each specs are define as function. 

### pecs, speciphy example

<pre><code>
namespace Speciphy\DSL;
return
describe('Bowling',
    describe('->score',
        context('all gutter game',
            subject(function () {
                $bowling = new Bowling;
                for ($i = 1; $i <= 20; $i++) {
                    $bowling->hit(0);
                }
                return $bowling;
            }),
            it('should equal 0', function ($bowling) {
                $bowling->score->should->equal(0);
            })
        )
    )
);
</code></pre>

Style of these frameworks became like a RSpec to use closure. It is cool. But closure (function()) is not cool, maybe.

### RSpec example

<pre><code>
describe Bowling, "#score" do
  it "returns 0 for all gutter game" do
    bowling = Bowling.new
    20.times { bowling.hit(0) }
    bowling.score.should eq(0)
  end
end
</code></pre>

Mmm...

### Spec for PHP

<pre><code>
describe "Bowling"
  describe "#score"
    it "returns 0 for all gutter game"
      $bowling = new Bowling;
      for ($i = 1; $i <= 20; $i++) {
        $bowling->hit(0);
      }
      $bowling->score should equal 0
    end
  end
end
</code></pre>

This is awesome !!
Is this PHP? really ??

## 3. Story frameworks

I think better story framework is cucumber.
We can select Behat or Cuke4PHP.
Behat is PHP version clone of famous BDD framework cucumber in Ruby on Rails.
Cuke4PHP is cucumber wire protocol with PHP. But it required ruby environment.

### feature 

* Added value of the feature
* Benefit of the feature

<pre>
In order to tell the masses what's on my mind
As a user
I want to read articles on the site
</pre>

Story framework basic define.
It looks like a user story.

<pre>
In order to [the benefit or value of the feature]
As a [the person (or role) who will benefit]
I want (or need) [some feature]
</pre>

### scenario

<pre>
Given [some initial context (the givens)]
When [an event occurs]
Then [ensure some outcomes]
</pre>

for example :

<pre>
  Scenario: Show the article
    Given I am on "TopPage"
    When  I follow "A title once again"
    Then  I should see "And the post body follows."
</pre>

## 4. BDD Plugin for CakePHP2

This is integration plugin for CakePHP2.
This plugin focuses on BDD for CakePHP application development. Like a Ruby on Rails, we can use 2 frameworks that are Story and Spec on CakePHP2. This plugin integrates to 

* Spec framework uses Spec for PHP.
* Story framework uses Behat.

I developed CakeBehat is integration to Behat. CakeBehat will move to BDD plugin as soon as.

### Require

* PHPUnit
* CakePHP 2.0 or later
* The data base such as MySQL must be installed, and the data base for the test must be prepared. 
* PHP 5.3.2 or later
* Composer

### Dependency installed by Composer.

* Behat and Mink (through MinkExtention)
* Mink goutte driver
* Mink selenium driver
* Spec for PHP 
  * Object_Freezer
  * Console_CommandLine
  * Hamcrest

I wrote Installation and configuration to README of the plugin.
And BddExampleApp have sample application include features and steps.
I wrote tutorial to README of the example application.
Please check these repository on github, because today is short time.

## 5. Testing Specification

BDD plugin provides spec shell for running tests. Options and arguments get along with Spec for PHP. From your root directory of CakePHP, you can do the following to run tests:
<pre>
# Run all tests into spec directory(default directory spec)
lib/Cake/Console/cake Bdd.spec

# You can be given target spec directory(for example spec)
lib/Cake/Console/cake Bdd.spec spec

# You can be given target spec file
lib/Cake/Console/cake Bdd.spec spec/post.spec.php

# For more options information is
lib/Cake/Console/cake Bdd.spec --help
</pre>

### Testing Models

You can write spec code:

    <?php

    # class CakeSpec
    describe "Post"
      context "with fixture"
        before
          $W->fixtures = array('app.post');
        end
        describe "#findByTitle"
          subject
            $post = ClassRegistry::init('Post');
            $result = $post->findByTitle('The title');
            return $result['Post']['body'];
          end
          it "body"
            should equal 'This is the post body.'
          end
        end
      end
    end

For testing model, must specify annotation # class CakeSpec.

In CakeSpec world variable $W->fixtures we define the set of fixtures. In the CakeTestCase, you should include all the fixtures at once. But CakeSpec needs only nested block. The fixtures are loaded each of "it" blocks (like a later load using loadFixtures method).

### Testing Controllers

    <?php

    # class ControllerSpec
    describe "PostsController"
      before
        $W->fixtures = array('app.post');
      end
      describe "/posts/index"
        subject
          $vars = \DrSlump\Spec::test()->testAction("/posts/index", array('return'=>'vars'));
          return array_map(function($post) { return $post['Post']['title']; }, $vars['posts']);
        end
        it 'have 3 titles'
          should equal array('The title', 'A title once again', 'Title strikes back')
        end
      end
    end

For testing controllers, must specify annotation # class ControllerSpec. ControllerSpec extends ControllerTestCase of CakePHP, so you can use testAction() and fixtures. Fixtures usage is similar in testing model, use $W->fixture.

## 6. Testing Story

BDD plugin provides story shell for running tests. Options and arguments get along with Behat. From your root directory of CakePHP, you can do the following to run tests:
<pre>
# Run all tests into features directory
lib/Cake/Console/cake Bdd.story

# You can be given target feature file
lib/Cake/Console/cake Bdd.story features/posts.features

# For more options information is
lib/Cake/Console/cake Bdd.story --help
</pre>

### feature

<pre>
# language: en
Feature: 
  In order to tell the masses what's on my mind
  As a user
  I want to read articles on the site

  Background:
    Given there is a post:
      | Title | Body |
      | The title | This is the post body. |
      | A title once again | And the post body follows. |
      | Title strikes back | This is really exciting! Not. |

  Scenario: Show the article
    Given I am on "TopPage"
    When  I follow "A title once again"
    Then  I should see "And the post body follows."
</pre>

### Dependency CakePHP

You may want to insert test data to database. In this case, BDD plugin provides 2 methods :

* truncateModel() : Method for deletion of test data. 
* getModel() Method of getting model to register test data. 

<pre>
$steps->Given('/^there is a post:$/', function($world, $table) {
  $hash = $table->getHash();
  $world->truncateModel('Post');
  $post = $world->getModel('Post');
  foreach ($hash as $row) {
    $post->create(array('Post'=>array('title'=>$row['Title'], 'body'=>$row['Body'])));
    $post->save();
  }
});
</pre>

## Demonstration




