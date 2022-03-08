# How to Create Windows-8-like animations with CSS3 and jQuery
This article was published on march, 2022, and takes approximately 16 minute(s) to read.

I have recently realized that CSS3 3D transforms have been out there for quite a long time now and yet I haven't experimented with them yet. I have also been using Windows 8 for a while now, and the first thing that struck me as impressive about it was the transitions and animations built into the dashboard, so I thought it would be really cool if my first experiment with CSS 3D transforms would be to recreate those animations and effects. So, here goes the tutorial on how I did that.

* Please note that this demo works only in browsers that support the CSS3 properties used.

* For the sake of brevity in the example code, I am using the un-prefixed CSS properties, but you will find the prefixes in the downloadeable source code on Github.

The Markup
The demo's structure is pretty simple: The dashboard is a list of tiles, of three sizes, small, big, and 2xbig, floated inside 3 columns. And then there are their corresponding "pages". A page is an overlay which opens when u click on one of the boxes on the dashboard. It represents an app on desktop, with each of the tiles being the shortcut to that app.

Each tile will open up a corresponding page. There are two kinds of page transitions included in the Windows 8 dashboard: one that opens the page in a 3D rotating effect from the right of the screen, and one that slides the page in and back to the left. We will define a class name for each type of page as s-page for the pages that slide from and to the left, and r-page for pages that rotate open from the right.

Now, for each tile, we need to specify what type of page it opens (depending on the effect you want for that page). We will define the type of the page for each tile in a custom data attribute called data-page-type, this will take care of applying the right class names triggering the right animations later on.

Each page should also have a name. The page name for a certain app will be different from that of another app, so the "Skype" tile opens up a page called "skype-app" for example. I've used only two page names in this example, which are repeated for all tiles, and used a custom-page name for the last tile for sake of example. You'll probably have to add a different page for each tile, hence a different page name specified in every tile.

Here's the markup for the whole dashboard (tiles and pages):
```html
?
<div class="demo-wrapper">
  <!-- classnames for the pages should include: 1) type of page 2) page name-->
   <div class="s-page random-restored-page">
      <div class="page-content">
        <h2 class="page-title">Some minimized App</h3>
        <div class="close-button s-close-button">x</div>
      </div>
    </div>
    <div class="s-page custom-page">
      <div class="page-content">
        <h2 class="page-title">Thank You!</h3>
        <div class="close-button s-close-button">x</div>
      </div>
    </div>
    <div class="r-page random-r-page">
      <div class="page-content">
        <h2 class="page-title">App Screen</h3>
        <p>Chew iPad power cord chew iPad power cord attack feet chase mice leave dead animals as gifts and stick butt in face chew iPad power cord. Chase mice. Run in circles use lap as chair why must they do that. Intrigued by the shower destroy couch leave hair everywhere sleep on keyboard chew iPad power cord. Use lap as chair. Missing until dinner time stand in front of the computer screen, intently sniff hand. Find something else more interesting. Destroy couch play time so inspect anything brought into the house hate dog burrow under covers. Sleep on keyboard destroy couch so hate dog so hide when guests come over. Chase mice destroy couch lick butt throwup on your pillow use lap as chair yet intrigued by the shower but climb leg. Stare at ceiling make muffins or hunt anything that moves claw drapes. Intently sniff hand intrigued by the shower. Why must they do that. Cat snacks leave dead animals as gifts or inspect anything brought into the house sweet beast so stare at ceiling give attitude. Flop over claw drapes but sun bathe lick butt, and chase mice. Rub face on everything lick butt leave hair everywhere lick butt, missing until dinner time for use lap as chair lick butt. Make muffins leave dead animals as gifts play time. Chew foot intrigued by the shower stare at ceiling inspect anything brought into the house yet hopped up on goofballs. 
 
        Hunt anything that moves intently sniff hand for hunt anything that moves play time. Chew foot climb leg throwup on your pillow so lick butt yet make muffins hate dog. Intrigued by the shower. Intently sniff hand shake treat bag. Cat snacks burrow under covers make muffins but all of a sudden go crazy find something else more interesting. Flop over chase mice. Give attitude. Inspect anything brought into the house. Stick butt in face sun bathe so find something else more interesting and intrigued by the shower. Rub face on everything use lap as chair. 
 
        Under the bed claw drapes chase mice but leave hair everywhere yet make muffins yet claw drapes. Use lap as chair. Find something else more interesting stretch for under the bed. Nap all day intrigued by the shower, hate dog sweet beast intently sniff hand so hate dog nap all day. Swat at dog hide when guests come over and mark territory chase mice for cat snacks. Use lap as chair. Lick butt throwup on your pillow need to chase tail. 
 
        Mark territory. Stick butt in face shake treat bag yet hunt anything that moves, yet hopped up on goofballs yet stare at ceiling under the bed. Give attitude chase imaginary bugs stretch so hunt anything that moves so hide when guests come over but intrigued by the shower find something else more interesting. Make muffins behind the couch for chew foot. Sweet beast flop over but throwup on your pillow. Intently sniff hand use lap as chair and missing until dinner time and chase imaginary bugs. 
        </p>
      </div>
       
      <div class="close-button r-close-button">x</div>
    </div>
  <!--each tile should specify what page type it opens (to determine which animation) and the corresponding page name it should open-->
    <div class="dashboard clearfix">
      <ul class="tiles">
        <div class="col1 clearfix">
          <li class="tile tile-big tile-1 slideTextUp" data-page-type="r-page" data-page-name="random-r-page">
            <div><p>This tile's content slides up</p></div>
            <div><p>View all tasks</p></div>
          </li>
          <li class="tile tile-small tile tile-2 slideTextRight" data-page-type="s-page" data-page-name ="random-restored-page">
            <div><p class="icon-arrow-right"></p></div>
            <div><p>Tile's content slides right. Page opens from left</p></div>
          </li>
          <li class="tile tile-small last tile-3" data-page-type="r-page" data-page-name="random-r-page">
            <p class="icon-calendar-alt-fill"></p>
          </li>
          <li class="tile tile-big tile-4" data-page-type="r-page" data-page-name="random-r-page">
            <figure>
              <img src="images/blue.jpg" />
              <figcaption class="tile-caption caption-left">Slide-out Caption from left</figcaption>
            </figure>
          </li>
        </div>
 
        <div class="col2 clearfix">
          <li class="tile tile-big tile-5" data-page-type="r-page" data-page-name="random-r-page">
            <div><p><span class="icon-cloudy"></span>Weather</p></div>
          </li>
          <li class="tile tile-big tile-6 slideTextLeft" data-page-type="r-page" data-page-name="random-r-page">
            <div><p><span class="icon-skype"></span>Skype</p></div>
            <div><p>Make a Call</p></div>
          </li>
          <!--Tiles with a 3D effect should have the following structure:
              1) a container inside the tile with class of .faces
              2) 2 figure elements, one with class .front and the other with class .back-->
          <li class="tile tile-small tile-7 rotate3d rotate3dX" data-page-type="r-page" data-page-name="random-r-page">
            <div class="faces">
              <div class="front"><span class="icon-picassa"></span></div>
              <div class="back"><p>Launch Picassa</p></div>
            </div>
          </li>
          <li class="tile tile-small last tile-8 rotate3d rotate3dY" data-page-type="r-page" data-page-name="random-r-page">
            <div class="faces">
              <div class="front"><span class="icon-instagram"></span></div>
              <div class="back"><p>Launch Instagram</p></div>
            </div>
          </li>
        </div>
 
        <div class="col3 clearfix">      
          <li class="tile tile-2xbig tile-9" data-page-type="custom-page" data-page-name="random-r-page">
            <figure>
              <img src="images/summer.jpg" />
              <figcaption class="tile-caption caption-bottom">Fixed Caption: Some Subtitle or Tile Description Goes Here with some kinda link or anything
              </figure>
          </li>
          <li class="tile tile-big tile-10" data-page-type="s-page" data-page-name="custom-page">
            <div><p>Windows-8-like Animations with CSS3 & jQuery © Sara Soueidan. Licensed under MIT.</p></div>
          </li>
        </div>
      </ul>
    </div><!--end dashboard-->
  </div>
  ```
The icon font I'm using is from Icomoon You can as well use fontawesome--this is the one i have used in the repo

What will happen is that the Javascript will get the name and type of page to be opened when a tile is clicked, and then, according to the type of the page, it will apply specific class names to the page (whose name we have also retrieved from the data-page-name attribute) to open it with the specified type of animation for each class applied.

# The CSS
Please note that I'm taking a mobile-first approach to the styles, which we'll then make responsive in the media queries section.

First, the styles for the demo wrapper, the container in which the whole demo will be contained. We'll define general styles, and make sure to set a perspective to activate the 3D space, otherwise, the whole demo will look flat and two dimensional.
```css
?
.demo-wrapper {
  padding: 2em .5em;
  width: 100%;
  height:100%;
  perspective: 3300px;
  position: relative;
}
Now let's start with the dashboard styles and animations.

The first animation applied to the dashboard is fired when the page loads. The dashboard is initially hidden and translated to the right of the screen, and fades and translates in to position on page load.

?
.dashboard {
  margin: 0 auto;
  width: 100%;
  padding: 1em;
  transform: translateX(200px);
  opacity:0;
  animation: start 1s ease-out forwards;
}
@keyframes start{
  0%{
    transform: translateX(200px);
    opacity:0;
  }
  50%{
    opacity:1;
  }
  100%{
    transform: translateX(0);
    opacity:1;
  }
}
```
The dashboard also fades into the view and fades back when a tile is clicked. Once a tile is clicked, the dashboard translates back along the z-axis, decreases in size, and fades its opacity gradually till it becomes 0. And when an opened page is closed, the dashboard fades back into the view.

The three columns in the dashboard fade in one after the other, with a slight delay between them. When a page is closed, a class name is added to each column (via Javascript), and each of these classes calls the animation with a certain delay.

Here are the classes and the animations applied to the dashboard upon clicking the tiles and closing the pages:
```css
?
.fadeOutback{
 animation: fadeOutBack 0.3s ease-out 1 normal forwards;
}
.fadeInForward-1, .fadeInForward-2, .fadeInForward-3 {
  /*remember: in the second animation u have to set the final values reached by the first one*/
  opacity:0;
  transform: translateZ(-5em) scale(0.75);
  animation: fadeInForward .5s cubic-bezier(.03,.93,.43,.77) .4s normal forwards;
}
 
.fadeInForward-2{
  animation-delay: .55s;
}
.fadeInForward-3{
  animation-delay: .7s;
}
 
@keyframes fadeOutBack{
  0% {transform: translateX(-2em) scale(1); opacity:1;}
  70% {transform: translateZ(-5em) scale(0.6); opacity:0.5;}
  95% {transform: translateZ(-5em) scale(0.6); opacity:0.5;}
  100% {transform: translateZ(-5em) scale(0); opacity:0;}
}
@keyframes fadeInForward{
  0% {transform: translateZ(-5em) scale(0); opacity:0;}
  100% {transform: translateZ(0) scale(1); opacity:1;}
}
Now we're going to style the pages.

?
.r-page {
  width: 100%;
  height: 100%;
  text-align: center;
  font-size: 2em;
  font-weight: 300;
  position: absolute;
  right: 0;
  top: 0;
  left:0;
  bottom:0;
  opacity: 0;
  color: white;
  z-index: 10;
  padding:10px;
  transform-origin: 100% 0%;
  transform: rotateY(-90deg) translateZ(5em)
}
 
.s-page {
  color: white;
  z-index: 10;
  text-align: center;
  font-size: 2em;
  font-weight: 300;
}
.page-content{
  overflow-y:auto;
  max-height:100%;
  font-size:.6em;
  padding:.6em;
  text-align:left;
}
 
.s-page, .r-page{
  background-color: white;
  color:black;
}
 
.page-title {
  margin: .25em 0;
  font-weight: 100;
  font-size: 3em;
  text-align:center;
}
 
.close-button {
  font-size: 1.5em;
  width: 1em;
  height: 1em;
  position: absolute;
  top: .75em;
  right: .75em;
  cursor: pointer;
  line-height: .8em;
  text-align: center
}
```
I've set the original position of each r-page in the 3D space by first rotating it about the y-axis (the vertical axis), then I moved the page 5em to the left of the screen by using translateZ. Always remember: when u transform an element in 3D, you transform its coordinate system along with it. What I wanted to do is move the page 5em to the left of the screen, but instead of using translateX I used translateZ, because after the first tranformation (rotation about y axis), the coordinate system was also rotated, so now the z-axis points towards the left, and the x-axis is pointing towards you, the viewer.

All the pages, except the s-page app page, have the same initial position in the 3D space. The s-pages, on the other hand, are positioned -150% left of the screen off canvas, so that they slide into view when their animation is fired.

Once the tile for each page is clicked, a corresponding class is added (via javascript) to the page that will open, and each class calls for a certain animation. So, each page will get a class name that will define the 3D effect for the page.

These are the class names that trigger the opening and closing of the pages, along with the animations defined for each class.
```css
?
/* opens the r-page type pages*/
.openpage{ 
  animation: rotatePageInFromRight 1s cubic-bezier(.66,.04,.36,1.03) 1 normal forwards;
}
/* closes the r-page type pages */
.slidePageLeft{
  transform: rotateY(0) translateZ(0) ; opacity:1;
  animation:slidePageLeft .8s ease-out 1 normal forwards;
}
/* opens the s-page type pages */
.slidePageInFromLeft{
  animation: slidePageInFromLeft .8s cubic-bezier(.01,1,.22,.99) 1 0.25s normal forwards;
}
/* closes the s-page type pages*/
.slidePageBackLeft{
  opacity:1; 
  left:0;
  animation: slidePageBackLeft .8s ease-out 1 normal forwards;
}
```
I'm using the animation shorthand property here. The last value forwards corresponds to the animation-fill-mode property, which must be set to forwards, otherwise the page will get back to its initial "closed" position after the animation is over. So, in order to keep the page open, and be able to create sequential animations, the element has to stay in the final state defined by the first animation, and from there start the second animation.

# These are the animations for the classes applied to the pages:
```css
?
@keyframes rotatePageInFromRight{
  0% {transform:rotateY(-90deg) translateZ(5em);opacity:0}
  30% {opacity:1}
  100% {transform: rotateY(0deg) translateZ(0) ; opacity:1}
}
 
/*When the close-button is clicked, the page slides to the left*/
/*note that the start of the second animation is the same state as the
end of the previous one*/
 
@keyframes slidePageLeft{
  0% {left:0; transform: rotateY(0deg) translateZ(0) ; opacity:1}
  70% {opacity:1;}
  100% {opacity:0; left:-150%; transform: rotateY(0deg)}
}
@keyframes slidePageInFromLeft{
  0% {opacity:0; }
  30% {opacity:1}
  100% {opacity:1; left:0;}
}
@keyframes slidePageBackLeft{
  0% {opacity:1; left:0; transform: scale(0.95);}
  10% {transform: scale(0.9);}
  70% {opacity:1;}
  100% {opacity:0; left:-150%;}
}
Last but not least we'll style the dashboard tiles and define the transitions and animations applied to them when they are hovered.

General styles defining the size of the tiles:

?
.tile{
  float: left;
  margin: 0 auto 1%;
  color: white;
  font-size: 1.3em;
  text-align: center;
  height: 8em;
  font-weight: 300;
  overflow: hidden;
  cursor: pointer;
  position:relative;
  background-color: #fff;
  color: #333;
  position:relative;
  transition: background-color 0.2s ease-out
}
.tile-2xbig{
  height:16.15em;
  width:100%;
}
.tile-big {
  width: 100%
}
.tile-small {
  width: 49%;
  margin-right: 2%
}
.tile-small.last {
  margin-right: 0
}
```
A couple of tiles contain an image along with an image caption. These tiles will get a class fig-tile to determine their type in the Javascript code. The colors used for the text and background of the corresponding page will be retrieved from the colors of the caption, so don't forget to define them.The caption can be either fixed, or it can slide in when the tile is hovered:
```css
?
.tile-caption{
  position:absolute;
  z-index:1;
  background-color: #455962;
  color:#fff;
  font-size:1em;
  padding:1em;
  text-align: left;
}
.caption-bottom{
  left:0;
  bottom:0;
  right:0;
  height:40%;
}
.caption-left{
  left:-100%;
  top:0;
  bottom:0;
  width:40%;
  transition: left .3s linear;
}
.tile:hover .caption-left{
  left:0;
}
```
Regular tiles, with no special kind of animation, will change their background and text color on hover. In order to make sure the text is vertically centered in each tile, each one will contain a div with a paragraph containing the text. We'll use the table-cell display property to center this text vertically.
```css
?
.tile div{
  position:absolute;
  top:0; left:0; right:0; bottom:0;
  width:100%;
  height:100%;
  text-align:center;
  display:table;
  padding:0 1em;
  transition: all .3s ease;
}
.tile div p{
  display:table-cell;
  vertical-align:middle;
}
```
I'll skip the general styles of the tiles for sake of brevity, but make sure you set a background and text color on all tiles, even the ones that will be covered by an image, because these colors will be retrieved via Javascript and set as the colors for the corresponding page of this tile. Let's move on to the animations and transitions on the tiles.

Tiles with text sliding inside of them will contain two divs, each div will be like a "face" or a block inside the tile. These divs are positioned absolutely, and moved on hover according to the direction of slide we want.

For a tile's text to slide up on hover, we'll apply a class slideTextUp.
```css
?
/* slide text inside tile up */
/* 2nd div will be positioned below the bottom of the tile*/
.slideTextUp div:nth-child(2){
  top:100%;
}
/*both divs will be translated up on hover*/
.slideTextUp:hover div{
  transform: translateY(-100%);
}
.tile-1 p{
  font-size:1.3em;
}
Similarly, tiles with text sliding to the right and left, will get class names slideTextLeft and slideTextRight, with a similar structure as the above tile.

?
/* slide text inside tile to the right*/
.slideTextRight div:first-child{
  left:-100%;
}
.slideTextRight:hover div{
  transform: translateX(100%);
} 
 
/* slide text inside tile to the left */
.slideTextLeft div:nth-child(2){
  left:100%;
}
.slideTextLeft:hover div{
  transform: translateX(-100%);
} 
```
A couple tiles have a different hover effect, they rotate to reveal the back face of the tile. This effect is a very simple and basic "card flip" effect. I won't get into the details of this effect, but if you're new to this, you can read more about it in this excellent tutorial by David De Sandro.

For this flipping effect, apply a rotate3d class to the tile you want to flip. For a card with a vertical flip we'll add a class rotate3dY, and for a horizontal flip we'll apply a class rotate3dX (in addition to the rotate3d class), and we'll apply the following styles:
```css
?
/* rotate tile in 3D*/
.rotate3d{
  /* add a perspective to the tile to activate 3d space inside it*/
  perspective: 800px;
  overflow: visible;
}
.faces{
  /* preserve the 3d space in the container wrapping the two "faces" and define a transition */
  transform-style: preserve-3d;
  transition: transform 1s;
}
.faces div {
  /* position faces on top of each other */
  display: block;
  position: absolute;
  top:0; left:0; right:0; bottom:0;
  width: 100%;
  height: 100%;
  /* hide backface visibility so that the back of the face is hidden when it's rotated */
  backface-visibility: hidden;
}
We'll rotate one of the faces in the tiles so that the two faces are back to back.

?
.rotate3dY .back{
  transform: rotateY( 180deg );
}
.rotate3dX .back{
  transform: rotateX( 180deg );
}
And when the tile is hovered, the .faces div will be rotated to reveal the back face.

?
.rotate3dY:hover .faces:hover{
    transform: rotateY( 180deg );
}
.rotate3dX:hover .faces:hover{
    transform: rotateX( 180deg );
}
```
For styles rotating in 3D, remember to set a background and text color for the .front face, so that these colors be retrieved and set to the tile's page when it is opened.

And that's all for the styles and animations!

Now let's define responsive styles for the dashboard. Dashboard columns are initially full-width on small screens (remember we're starting mobile-first), and they will be floated next to each other on big screens.
```css
?
.col1,
.col2,
.col3 {
  width: 99%;
  margin: 1em auto
}
@media screen and (min-width: 43.75em) {
  .col1,
  .col2,
  .col3 {
    float: left;
    margin-right: 1%;
    width: 49%
  }
  .page-title{
    font-size:2.5em;
  }
  .page-content{
    font-size:1em;
  }
  .close-button{
    font-size:2em;
  }
}
 
@media screen and (min-width: 64em) {
  .col1,
  .col2,
  .col3 {
    float: left;
    margin-right: .5%;
    width: 31%
  }
 
  .col3 {
    margin-right: 0
  }
 
  .col1 {
    margin-left: 2em
  }
  .page-title{
    font-size:3.5em;
  }
}
```
# The Javascript
All click events will be handled wtih Javascript. I'll be using jQuery for this example. Event handlers are going to be set on each of the dashboard tiles, and when a click event is detected, we're going to retrieve the name and type of the corresponding page from the data-page-type and data-page-name attributes, and use these to open the page.

Other click events will be handled when clicking on the close button in each page. The close button for each page type will apply the suitable class names to close this page type.

Additionally, in order to give each page the same background color and text color as its corresponding tile, we will first loop through the tiles, retrieve its colors, and then applies it to its corresponsing page. If a tile has a rotate3d class, it looks for the background-color of the front "face" of the tile, and applies that to the page.
```php
?
(function(){
    //get the background-color for each tile and apply it as background color for the cooresponding screen
    $('.tile').each(function(){
        var $this= $(this),
            page = $this.data('page-name'),
            bgcolor = $this.css('background-color'),
            textColor = $this.css('color');
        //if the tile rotates, we'll use the colors of the front face
            if($this.hasClass('rotate3d')) {
              frontface = $this.find('.front');
              bgcolor = frontface.css('background-color');
              textColor = frontface.css('color');
            }
        //if the tile has an image and a caption, we'll use the caption styles
            if($this.hasClass('fig-tile')) {
              caption = $this.find('figcaption');
              bgcolor = caption.css('background-color');
              textColor = caption.css('color');
            }
 
        $this.on('click',function(){
          $('.'+page).css({'background-color': bgcolor, 'color': textColor})
                     .find('.close-button').css({'background-color': textColor, 'color': bgcolor});
        });
    });
 
    function showDashBoard(){
      for(var i = 1; i <= 3; i++) {
        $('.col'+i).each(function(){
            $(this).addClass('fadeInForward-'+i).removeClass('fadeOutback');
        });
      }
    }
 
    function fadeDashBoard(){
      for(var i = 1; i <= 3; i++) {
        $('.col'+i).addClass('fadeOutback').removeClass('fadeInForward-'+i);
      }
    }
   
     
  //listen for when a tile is clicked
  //retrieve the type of page it opens from its data attribute
  //based on the type of page, add corresponding class to page and fade the dashboard
  $('.tile').each(function(){
    var $this= $(this),
        pageType = $this.data('page-type'),
        page = $this.data('page-name');
         
    $this.on('click',function(){
      if(pageType === "s-page"){
          fadeDashBoard();
          $('.'+page).addClass('slidePageInFromLeft').removeClass('slidePageBackLeft');
        }
        else{
          $('.'+page).addClass('openpage');
          fadeDashBoard();
        }
    });
  });
 
  //when a close button is clicked:
  //close the page
  //wait till the page is closed and fade dashboard back in
  $('.r-close-button').click(function(){
      $(this).parent().addClass('slidePageLeft')
          .one('webkitAnimationEnd oanimationend msAnimationEnd animationend', function(e) {
                $(this).removeClass('slidePageLeft').removeClass('openpage');
              });
      showDashBoard();
  });
  $('.s-close-button').click(function(){
      $(this).parent().removeClass('slidePageInFromLeft').addClass('slidePageBackLeft');
      showDashBoard();
  });
 
})();
```
# And that's it!
I hope you enjoyed this tutorial and found it useful! :) 