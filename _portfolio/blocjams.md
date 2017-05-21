---
layout: post
title: BlocJams
thumbnail-path: "/img/landing.png"
short-desctiption: BlocJams is a spotify replica for playing music.

---

{:.center}
![]({{site.baseurl}}/img/landing.png)

## Summary

The new digital world has allowed us to experience our music collection on the go in ways like never before. Bloc Jams looks to build upon and enhance the user’s interaction with music. 

## Explanation 

This project was done under the guidance of the Bloc curriculum. My role in the project was to create the media interface in which users can look at their music libraries and play songs seamlessly. 

## Problem

Creating a music site brings forth speed and efficiency challenges as reloading of static pages can create a less desirable user experience. Users want seamless interaction with their music and having page reloads can bog down the site and create a slow and unresponsive effect.

## Solution

AngularJs provides the perfect solution to slow page reloads. Angular allows us to insert different pages of the site all in one place without the site reloading. Through the use of controllers, services and routes we are able to perform different actions on a single page. This helps the site feel more like a native machine app then a website thus improving the user experience. For developers, using Angular helps make our code much more modular and reusable. 

In addition, Angular allows us to have two-way data binding, meaning that changes made to the model(database) would be reflected almost simultaneously in our view that the user sees. Therefore, our users can have real-time interaction with the database.


## Results

The use of angular within our web app gave us fantastic results, as the site was much quicker to load up the collections of albums and the music player. 

![]({{site.baseurl}}/img/collection.png)
![]({{site.baseurl}}/img/album.png)


>Having created individual html templates for our specific pages helped us with modularity as well as provided us with an easier and faster way of loading all of the pages in the index.html once the appropriate route was selected. For example to view the collection of albums in our site, we need to navigate to base-url/collection. Doing this the old way via static pages would cause the browser to reload and show the appropriate html and css for that page. However, with the use of templates and routes we are able to navigate without any page reload and the transition is much smoother like a native app. 

>The use of two-way data binding shows up in the loading of both the collection of albums as well as the list of songs in each album. By writing minimal code we are able to show the list of albums from our database in the view. 

```html
<section class="album-covers container clearfix">
     <div class="collection-album-container column fourth" ng-repeat="album in collection.albums track by $index">
         <img src="{{album.albumArtUrl}}"/>
         <div class="collection-album-info caption">
             <p>
                 <a class="album-name" ui-sref="album">{{album.title}}</a>
                 <br/>
                 <a ui-sref="album">{{album.artist}}</a>
                 <br/>
                 {{album.songs.length}} songs
                 <br/>
             </p>
         </div>
     </div>
</section>
```

```javascript
(function (){
	function CollectionCtrl(Fixtures){
		this.albums = Fixtures.getCollection(12);
	}
	angular
		.module('blocJams')
		.controller('CollectionCtrl',['Fixtures', CollectionCtrl]);
})();
```

## Conclusion 

I was really happy with the way this project turned out. Angular provided me with a very effect solution to making the Bloc Jams app work like a native app. The use of routes, controller and templates made the project much easier to put together. I learned a lot about the power and versatility of angular in handling all the interfaces of the web app. Angular will continue to play a major role in my future projects and I look forward to using it again. 

