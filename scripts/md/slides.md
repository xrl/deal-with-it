
title: Deal With It
class: big

<h1>Memeing your screen since 2005</h1>
<p style="text-align: center">
  <img src="/images/dwi/Deal_with_it_dog_gif.gif"/>
</p>

---

title: Elements of a 'Deal With It'

- Conflict and/or Attitude
- Sunglasses
- "Deal With It"

---

title: <em>Technical</em> Elements of a 'Deal With It'
class: big

- Static or animated base image
- Static or animated sunglasses
- Static or animated text

---

title: Exhibit A
subtitle: Something with tines

<p style="text-align: center">
  <img src="/images/dwi/cactus.gif"/>
</p>

---

title: Exhibit B
subtitle: Spoiler Alert

<p style="text-align: center">
  <img src="/images/dwi/data-vs-borg.gif" width="80%" style="margin: auto"/>
</p>

---

title: Exhibit C
subtitle: Spoiler Alert (II)

<p style="text-align: center">
  <img src="/images/dwi/godzilla.gif" width="80%" style="margin: auto"/>
</p>

---

title: Exhibit D
subtitle: Internet is for Cats

<p style="text-align: center">
  <img src="/images/dwi/cat-skateboard.gif" width="80%" style="margin: auto"/>
</p>

---

title: Graphics Interchange Format
class: big

- 256 colors per frame
- Animation
- Pronunciation: "Choosy developers choose GIF"

---

title: JPEG
subtitle: Just an FYI

- No animations
- Way more complex

---

title: What We're Doing Today

- Drawing
- Animating
- Exporting

---

title: Drawing
class: big

<div style="width: 100%">
  <div class="pillar black gray-bg">
    <h1>Canvas</h1>
    <ul>
      <li>Pixels at (X,Y) or (X,Y,Z)</li>
      <li>Poor Text Rendering</li>
      <li>No animation support</li>
      <li>GIF exporting libraries support it</li>
    </ul>
  </div>
  <div class="pillar black green-bg">
    <h1>SVG</h1>
    <ul>
      <li>Scaleable</li>
      <li>Good text support</li>
      <li>Animations</li>
      <li>DOM elements</li>
      <li>Slows down with DOM complexity</li>
      <li>Did not find GIF exporting library support</li>
    </ul>
  </div>
</div>


---

title: And the winner is...

<span class="blue" style="font-size: 60pt">Canvas!<span>

---

title: Canvas Demo


<a class="jsbin-embed" href="http://jsbin.com/iwovaj/74/embed?js,output">Ember Starter Kit</a><script src="http://static.jsbin.com/js/embed.js"></script>

---

title: How does canvas animation work?

- Render the whole canvas
- Wait
- GOTO 1

---

title: So... how are we building an app?

- Manage (X,Y) of the animation path
- Animate sunglasses
- Add base image
- Render text
- Let the web browser handle frame swapping

---

title: Manage (X,Y) of the animation path

- Track mouse location in DOM element
- Keep track of locations over time

---

title: Manage (X,Y) (code I)

<pre class="prettyprint" data-lang="javascript">
  steps: [],
  isMouseDown: false,
  mouseDown: function(){
    this.set('isMouseDown',true);
  },
  mouseUp: function(){
    this.set('isMouseDown',false);
  },
  mouseMove: function(event,view){
    if(this.get('isMouseDown')){
      var point = this.relMouseCoords(event,this.get('canvas'));
      // point = {x: XX, y: YY}
      this.get('steps').pushObject(point);
      this.drawLine();
    }
  }
</pre>

---

title: Manage (X,Y) (code II)

<pre class="prettyprint" data-lang="javascript">
  // http://jsbin.com/ApuJOSA/1/edit
  relMouseCoords: function(e) {
    var el=e.target, c=el;
    var scaleX = c.width/c.offsetWidth || 1;
    var scaleY = c.height/c.offsetHeight || 1;

    if (!isNaN(e.offsetX))
      return { x:e.offsetX*scaleX, y:e.offsetY*scaleY };

    var x=e.pageX, y=e.pageY;
    do {
      x -= el.offsetLeft;
      y -= el.offsetTop;
      el = el.offsetParent;
    } while (el);
    return { x: x*scaleX, y: y*scaleY };
  }
</pre>

---

title: Manage (X,Y) Draw Line

<pre class="prettyprint" data-lang="javascript">
  drawLine: function(){
    var lineArr = this.get('steps');
    
    var cvas = this.get('canvas');
    var ctx = cvas.getContext("2d");
    ctx.beginPath();
    for(var i=0; i&lt;lineArr.length; i++){
      var point = lineArr[i];
      ctx.lineTo(point.x,point.y);
    }
    ctx.stroke();
  }
</pre>

---

title: Manage (X,Y) Demo

http://emberjs.jsbin.com/gohix/1/edit

---

title: Animate Sunglasses

- Load image in to DOM
- Copy in to canvas
- Track the mouse

---

title: Load image in to the DOM

<pre class="prettyprint" data-lang="html">
  &lt;img id=&quot;sunglasses&quot;
          src=&quot;https://s3-us-west-1.amazonaws.com/xrl/talks/sdjs-deal-with-it/sunglasses.png&quot;
          style=&quot;display: none&quot;/&gt;
</pre>

---

title: Copy in to canvas

<pre class="prettyprint" data-lang="javascript">
var img = $("img#sunglasses");
var canvas = $("canvas#deal-with-it");
var ctx = canvas.getContext("2d");

ctx.drawImage(img,
              point.x, point.y,
              img.width*scale,
              img.height*scale);
</pre>

---

title: Track the mouse

<pre class="prettyprint" data-lang="javascript">
mouseMove: function(event,view){
  if(this.get('isMouseDown')){
    var point = this.relMouseCoords(event,this.get('canvas'));
    this.get('steps').pushObject(point);
    this.drawGlasses(point);
  }
},
drawGlasses: function(point){
  var img = this.get('sunglasses'), scale = this.get('sunglassesScale');
  var cvas = this.get('canvas');
  var ctx = cvas.getContext("2d");
  
  ctx.drawImage(this.get('sunglasses'),point.x,point.y,img.width*scale,img.height*scale);
}
</pre>

- http://emberjs.jsbin.com/cayix/1/edit

---

title: Uh, not what we wanted

- Additive
- Clear the canvas with clearRect!

---

title: Track the mouse (now with clearing)

<pre class="prettyprint" data-lang="javascript">
mouseMove: function(event,view){
  if(this.get('isMouseDown')){
    var point = this.relMouseCoords(event,this.get('canvas'));
    this.get('steps').pushObject(point);
    this.drawGlasses(point);
  }
},
drawGlasses: function(point){
  var img = this.get('sunglasses'), scale = this.get('sunglassesScale');
  var cvas = this.get('canvas');
  var ctx = cvas.getContext("2d");
  
  context.clearRect(0, 0, cvas.width, cvas.height);
  ctx.drawImage(this.get('sunglasses'),point.x,point.y,img.width*scale,img.height*scale);
}
</pre>

http://emberjs.jsbin.com/cayix/4/edit

---

title: Draw the background

- Replaces ctx.clearRect()
- Load up an image from the DOM... of me!
- Don't mind the EmberJS'ism

<pre class="prettyprint" data-lang="html">
  &lt;img id=&quot;sunglasses&quot;
          src=&quot;https://s3-us-west-1.amazonaws.com/xrl/talks/sdjs-deal-with-it/xavier-deuces.png&quot;
          style=&quot;display: none&quot;/&gt;
</pre>

<pre class="prettyprint" data-lang="javascript">
  drawBackground: function(){
    var cvas = this.get('canvas');
    var ctx = cvas.getContext("2d");

    ctx.drawImage(this.get('background'),0,0);
  }.on('didInsertElement')
</pre>

---

title: Layer the canvas

<pre class="prettyprint" data-lang="javascript">
  if(this.get('isMouseDown')){
    this.drawBackground();

    var point = this.relMouseCoords(event,this.get('canvas'));
    this.get('steps').pushObject(point);
    this.drawLine();
    this.drawGlasses(point);
  }
</pre>

---

title: Playback

<pre class="prettyprint" data-lang="javascript">
  frameTimers: [],
  animationSpeed: 30,
  startAnimation: function(){
    this.drawBackground();
    var steps = this.get('steps'), frame=0;
    var self = this;
    
    for(var i=0; i &lt; steps.length; i=i+1){
      var timer = window.setTimeout(function(){
        var point = steps[frame];
        self.drawBackground();
        self.drawLine();
        self.drawGlasses(point);
        frame = frame + 1;
      }, self.get('animationSpeed')*i);
      
      self.get('frameTimers').pushObject(timer);
    }
  }
</pre>

http://emberjs.jsbin.com/guhit/3/edit

---

title: Canceling Playback

<pre class="prettyprint" data-lang="javascript">
  cancelAnimation: function(){
    this.get('frameTimers').forEach(window.clearTimeout);
    this.set('frameTimers',[]);
    this.drawBackground();
  }
</pre>

http://emberjs.jsbin.com/guhit/3/edit

---

title: Drawing Text

<pre class="prettyprint" data-lang="javascript">
  displayText: "Deal With It",
  font: "40pt Arial",
  setFont: function(font){
    var cvas = this.get('canvas');
    var ctx  = cvas.getContext('2d');
    ctx.font = font;
  },
  drawText: function(){
    var text = self.get('displayText');
    self.setFont(self.get('font'));
    self.drawText(text,400,50);

    var cvas = this.get('canvas');
    var ctx  = cvas.getContext('2d');
    
    ctx.strokeText.apply(ctx,arguments);
  }
</pre>

http://emberjs.jsbin.com/gujon/73/edit

---

title: Exporting GIF

- https://github.com/jnordberg/gif.js

<pre class="prettyprint" data-lang="javascript">
  var gif = new GIF({
    workers: 2,
    quality: 10
  });

  // or a canvas element
  gif.addFrame(canvasElement, {delay: 200});

  // or copy the pixels from a canvas context
  gif.addFrame(ctx, {copy: true});

  gif.on('finished', function(blob) {
    window.open(URL.createObjectURL(blob));
  });

  gif.render();
</pre>

---

title: Replaying animation in to gif.js

<pre class="prettyprint" data-lang="javascript">
  generateGIF: function(){
    var gif = new GIF({quality: 10}),
        steps = this.get('steps'),
        canvas = this.get('canvas');
    for(var i=0; i &lt; steps.length; i=i+1){
      var point = steps[i];
      this.drawBackground();
      this.drawLine();
      this.drawGlasses(point);
      gif.addFrame(canvas, {delay: this.get('animationSpeed'),
                            copy: true});
    }
    
    var self=this;
    gif.on('finished',function(blob){
      window.open(URL.createObjectURL(blob));
    });
    gif.render();
  }
</pre>

---

title: Canvas serializations fails...

- Need to work around security problems
- Host all assets on same domain and from web

---

title: Final version

- Bells and whistles from EmberJS
- get/set are the weirdest
- otherwise ignore

http://emberjs.jsbin.com/gujon/73/edit
