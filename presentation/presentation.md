title: Rails Basics 02 : Views
author: Haris Dimitriou (xarisd)
description: An introduction to Views part of the MVC in Rails while building a simple web application
date: <%= Date.today %>
% available themes: Default - Sky - Beige - Simple - Serif - Night - Moon - Solarized
theme: Simple
% available transitions: // default/cube/page/concave/zoom/linear/fade/none
transition: linear
custom_css: presentation
% code-engine: coderay

# Rails Basics 02 : Views
<p>&nbsp;</p>
<p class="fragment">Using Rails Views the proper way</p>

!SLIDE
## Who am I?
<p>&nbsp;</p>
<h3 class="fragment">
  Haris Dimitriou (<strong>xarisd</strong>)
</h3>
<p>&nbsp;</p>
<p class="fragment">
  Ruby developer<span class="fragment">...among other things</span>
</p>
<p>&nbsp;</p>
<p class="fragment">
  CTO and co-founder @<a href="http://www.polyptychon.gr">Polyptychon</a> (<a href="http://polyptychon.gr">polyptychon.gr</a>)
</p>
<p>&nbsp;</p>
<p class="fragment">
  <a href="http://github.com/xarisd">github.com/xarisd</a>
  <br>
  <a href="http://twitter.com/xarisd">twitter.com/xarisd</a>
  <br>
  <a href="http://xarisd.io">xarisd.io</a>
</p>


!SLIDE
## Agenda

<p>&nbsp;</p>

* MVC Recap
* Asset Pipeline : the very basics
* Adding CSS and JavaScript to the Rails App
* Layouts
* Using Layouts in Controllers and Views
* Passing data from Controller to View


!SLIDE
## Things we will not cover

<p>&nbsp;</p>

* Partials
* View Helpers
* Reading data from JSON for the "Members" page
* Rendering semi-static pages from Markdown like the "Home" page
* Advanced Layouts and Partials
* Advanced Asset Pipeline
* Controller filters and advanced concepts
* Advanced Routing (REST, nested routes etc)
* Models and Active Record
* Background Jobs
* ...

<p class="fragment">maybe next time(s)...</p>

!SLIDE down-open
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
!SLIDE

## MVC : Recap

<% left do %>
  <img class="fragment" src="images/MVC-Diagram.png" alt="MVC Diagram" width="550px" style="border: none;" />
<% end %>
<% right do %>
  <ol>
    <li class="fragment">Browser makes a <code>request</code></li>
    <li class="fragment"><code>Controller</code> recieves the request</li>
    <li class="fragment"><code>Controller</code> talks with the <code>Model(s)</code></li>
    <li class="fragment"><code>Controller</code> prepares the <code>View</code> for rendering</li>
    <li class="fragment"><code>Response</code> (HTML, JSON, etc) is returned to the Browser</li>
  </ol>
<% end %>



!SLIDE down-close
!SLIDE down-open
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

!SLIDE

## Asset Pipeline

!SLIDE

## What is the Asset Pipeline?

<p>&nbsp;</p>

<ul>
  <li class="fragment">Asset pipeline provides a framewrok to <code>concatenate</code> and <code>minify</code> <code>JavaScript</code> and <code>CSS</code> assets</li>
  <li class="fragment">Adds the ability to <code class="fragment">use pre-processors</code> <span class="fragment">such as <code>CoffeScript</code> <code>Sass</code> and <code>ERB</code></span></li>
  <li class="fragment">It is technically the <code>sprockets-rails</code> gem</li>
  <li class="fragment">It is enabled by default</li>
</ul>


!SLIDE

#### Asset Pipeline

## Concatenation
<p>&nbsp;</p>
<p class="fragment">Concatenates all JavaScript files into one master .js </p>
<p>&nbsp;</p>
<p class="fragment">Concatenates all CSS files into one master .css </p>
<p>&nbsp;</p>
<p class="fragment">Result: <span class="fragment"><code>Reduce the number of requests</code> to render a web page</span></p>

!SLIDE

#### Asset Pipeline

## Minification (or Compression)
<p>&nbsp;</p>
<p class="fragment">For CSS files, this is done by removing whitespace and comments</p>
<p>&nbsp;</p>
<p class="fragment">For JavaScript, more complex processes can be applied</p>
<p>&nbsp;</p>
<p class="fragment">Result: <span class="fragment"><code>Reduce the size of the files</code></span></p>

!SLIDE

#### Asset Pipeline

## Pre-processing
<p>&nbsp;</p>
<p class="fragment">It allows coding assets via a higher-level language, with precompilation down to the actual assets</p>
<p>&nbsp;</p>
<p class="fragment">Supported languages include <code>Sass</code> for CSS, <code>CoffeeScript</code> for JavaScript, and <code>ERB</code> for both by default</p>
<p>&nbsp;</p>
<p class="fragment">Result: <span class="fragment"><code>Ease of development</code></span></p>



!SLIDE

#### Asset Pipeline

## Fingerpriting
<p>&nbsp;</p>
<p class="fragment">Fingerprinting is a technique that makes the name of a file dependent on the contents of the file</p>
<p>&nbsp;</p>
<p class="fragment">When the file contents change, the filename is also changed</p>
<p>&nbsp;</p>
<p class="fragment">For content that is static or infrequently changed, this provides an easy way to tell whether two versions of a file are identical, even across different servers or deployment dates</p>
<p>&nbsp;</p>
<p class="fragment">When a filename is unique and based on its content, HTTP headers can be set to encourage caches everywhere</p>

!SLIDE

#### Asset Pipeline

## Fingerpriting (2)

<p>&nbsp;</p>
<p class="fragment">When the content is updated, the fingerprint will change</p>
<p>&nbsp;</p>
<p class="fragment">This will cause the remote clients to request a new copy of the content</p>
<p>&nbsp;</p>
<p class="fragment">The technique sprockets uses for fingerprinting is to insert a hash of the content into the name, usually at the end</p>
<p>&nbsp;</p>
<p class="fragment">Result: <span class="fragment"><code>Great caching strategy</code> for assets</span></p>


!SLIDE

## Asset Pipeline
<p>&nbsp;</p>
<p class="fragment">Use it!</p>
<p>&nbsp;</p>
<p class="fragment">It is really good!</p>

!SLIDE

## Asset Pipeline
<p>&nbsp;</p>
<p class="fragment">Demo time...</p>

!SLIDE down-close
!SLIDE down-open
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

!SLIDE

## Rails Views
<p>&nbsp;</p>
<p class="fragment">The final HTML output is a composition of three Rails elements</p>
<p>&nbsp;</p>
<p class="fragment">Templates</p>
<p>&nbsp;</p>
<p class="fragment">Partials</p>
<p>&nbsp;</p>
<p class="fragment">Layouts</p>

!SLIDE
## Templates
<p>&nbsp;</p>
<p class="fragment">Action View templates can be written in several ways</p>
<p>&nbsp;</p>
<p class="fragment">If the template file has a <code>.erb</code> extension then it uses a mixture of <code>ERB</code> (included in Ruby) and <code>HTML</code></p>
<p>&nbsp;</p>
<p class="fragment">If the template file has a <code>.builder</code> extension then a fresh instance of <code>Builder::XmlMarkup</code> library is used</p>
<p>&nbsp;</p>
<p class="fragment">Rails supports multiple template systems and uses a file extension to distinguish amongst them</p>
<p>&nbsp;</p>
<p class="fragment">By default, Rails will compile each template to a method in order to render it (in production mode)</p>

!SLIDE
## Partials
<p>&nbsp;</p>
<p class="fragment">Partial templates are another device for breaking the rendering process into more manageable chunks</p>
<p>&nbsp;</p>
<p class="fragment">With partials, you can extract pieces of code from your templates to separate files and also reuse them throughout your templates</p>
<p>&nbsp;</p>
<p class="fragment">We will not cover them today</p>



!SLIDE
## Layouts
<p>&nbsp;</p>
<p class="fragment">Layouts can be used to render a common view template around the results of Rails controller actions</p>
<p>&nbsp;</p>
<p class="fragment">Typically, every Rails application has a couple of overall layouts that most pages are rendered within</p>

!SLIDE
## Layouts
<p>&nbsp;</p>
<p class="fragment">Demo time...</p>

!SLIDE down-close
!SLIDE down-open
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
!SLIDE
## Things we will cover **next time**

<p>&nbsp;</p>

* Partials
* View Helpers
* Rails Views Best Practices
* Reading data from JSON for the "Members" page
* Rendering semi-static pages from Markdown like the "Home" page

<p>&nbsp;</p>
<p class="fragment">until next time...</p>

!SLIDE down-close

!SLIDE down-open
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
!SLIDE
## Thank you!
<p>&nbsp;</p>
<p class="fragment">Liked the presentation?</p>
<p>&nbsp;</p>
<p class="fragment">
  Source: <a href="http://github.com/xarisd/rails-basics-02-views"> github.com/xarisd/rails-basics-02-views</a>
</p>
<p>&nbsp;</p>
<p>
<p class="fragment">
  View it online: <a href="http://xarisd.io/presentations/rails-basics-02-views">xarisd.io/presentations/rails-basics-02-views</a>
</p>
<p>&nbsp;</p>
<p class="fragment">
  Have something to say?
</p>
<p class="fragment">
  Send me a tweet or direct message: <a href="http://twitter.com/xarisd">@xarisd</a>
</p>
<p>&nbsp;</p>
<p class="fragment">Questions?</p>
