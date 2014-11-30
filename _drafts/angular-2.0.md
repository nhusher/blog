---

title: Angular 2.0: A Closer Look

layout: post

---

The big news in the web world is the (re?)-announcement of Angular 2.0. There was a lot of noise about it in the early part of this year, but very little concrete information was released about it. They were going to write in ES6 and compile to ES5, rely heavily on `Object.observe`, and were looking to rethink some of key pain points inside of Angular. Regardless, a shakeup was on the horizon.

The most recent annoucement was surprising for a couple reasons.

The first is that it represents a big departure from the limited information available in March: Angular is going to be written in [AtScript], a set of syntax extensions on top of ES6 that allow type information and Java-style annotations. It's going to totally reject some of the constructs that Angular apps are currently built around, `$scope`, controllers, modules, etc.

The second is the *way* that these things were announced, which seemed incredibly insensitive to me. Instead of presenting the new architecture in a value-neutral way, it was presented as a series of constructs in Angular 1.x that Angular 2.0 "killed". This, coupled with the message of "no upgrade path," and 1.x gaining no new significant features, sent many Angular users into a frenzy.

For good reason -- most software developers aren't big fans of the frameworks they use, and would rather not deal with large framework changes. Developers use frameworks because they help create useful software without engaging in much [yak shaving]. Most developers will continue to use a framework as long as the number of pointless tasks the framework creates for them is less than the pointless tasks *not* having a framework would create for them. A huge change to the framework API generates an enormous amount of work without solving any new problems for their users.

I'm on the outside of this particular clusterfuck, and for a few reasons I've been feeling a little [schadenfruede] about the whole thing. I'm not Angular's biggest fan for a number of reasons that I'll probably write about one day, and my pervasive contrarian streak makes me view anything popular in JS as something to be scrutinized very carefully. I also perceive the development team to be incredibly cavalier; in versions 1.0 through 1.2 (I haven't messed with 1.3 yet), there were changes to the API that broke compatibility in significant and subtle ways both. The 2.0 announcement reinforces this perception significantly.

I think that covers the optics of Angular 2.0, and my personal biases. But what about the content?

#### Angular is doing the right thing

While I don't have an extensive understanding of everything that's changing for Angular 2.0, from what I've seen, it's well-designed, forward-looking, and has a lot of interesting thoughts folded into it. The first thought when I looked at it was, "this looks like AppKit/UIKit." Objects bind to views in a similar way to Cocoa-style view controllers. Obviously, there are some significant differences, but the object-view relationshp is very similar.

Advancing the cause of ES6 (through the lens of AtScript, which I'll talk about later) is a good one. I've written code using ES6 modules, arrow functions, generators, and destructuring statements&mdash;it's difficult to go back once you've had a taste. Code written with new ES6 syntax can be much more expressive, easier to maintain, and easier to compose into a working application. Also, having one of the most popular frameworks moving in that direction should get ES6 prioritized for current browser makers.

Speaking of which, the browser landscape is changing extremely rapidly these days. Persistently bad browsers (old versions of IE, very old versions of Firefox, and lousy smartphone browsers) make up a small fraction of the market instead of overwhelmingly dominating it. As of this writing, IE7-10 accounts for somewhere around 10% of the North American browser market. IE11 and recent versions of both Chrome and Firefox are "evergreen," which should speed the adoption of ES6 and other fancy browser features.

From a timeline perspective, announcing early allows development teams to plan and isolate framework thrash. If Angular 2.0 will be released sometime late in 2015, a development organization can plan for a transition sometime in early 2016. It also puts development teams on notice that major constructs of the framework are disappearing, which allows them to step back from those constructs and consider writing code that's less coupled to the Angular idioms.

Lastly, major popular frameworks redesign their API and development practices fairly frequently with great success. Few are as ambitious as Angular, but the idea isn't without precedent. The transition from YUI2 to YUI3 worked pretty well in retrospect, and Angular may follow a similar course.

#### Angular is doing the wrong thing

Let's look at where things are going wrong, though. The biggest problem I see is that the last six months have been radio silence with regard to Angular 2.0. 

2. The Angular team did a bad thing
    * They've been working on this for 6 months with complete radio silence
    * They're not committed to an upgrade path
    * It's encouraging a compile-to solution
    * It's not targeting things as they are today, but as they may be eventually
    * http://kangax.github.io/compat-table/es6/ - 



Are the changes to Angular a good idea? Are those ideas being executed on effectively? Have other framework maintainers made similar moves in the past, and how did they turn out?
    
    
[AtScript]: http://www.theregister.co.uk/2014/11/04/improving_javascript_google_throws_atscript_into_the_mix/
[yak shaving]: http://www.catb.org/jargon/html/Y/yak-shaving.html
[schadenfruede]: http://blog.founddrama.net/2014/10/optimize-for-trade-offs/

http://jaxenter.com/angular-2-0-announcement-backfires-112127.html
https://www.youtube.com/watch?v=gNmWybAyBHI
http://www.michaelbromley.co.uk/blog/267/my-thoughts-on-ngeurope-2014-and-angularjs-2-0
http://www.infoq.com/news/2014/10/angular-2-atscript
http://angularjs.blogspot.fi/2014/10/ng-europe-angular-13-and-beyond.html
https://docs.google.com/document/d/1dZdq2L8EkzimgvU93ypLF9GJpdzD2jjm08Zal6sfxMQ/edit?hl=en&forcehl=1
https://medium.com/@angularjsdev/less-angularjs-more-javascript-ab756cfb81
https://plus.google.com/app/basic/stream/z12cclbotoypvn1uz23ztzmqcor3fzen2
http://codebetter.com/johnvpetersen/2014/10/27/how-google-broke-the-oss-compact-with-angular-2-0/
https://medium.com/@jeffwhelpley/screw-you-angular-62b3889fd678

---

http://larseidnes.com/2014/11/05/angularjs-the-bad-parts/