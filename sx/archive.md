---

title: Archive
permalink: /archive/
---

<html>
<head>
<meta charset="utf-8">
<title></title>

<script src="/bheema/js/jquery.min.js"></script>
<link rel="stylesheet" href="/bheema/css/main.css">

<style>
html, body {
  margin: 0;
  padding: 0;
  height: 100%;
}
#fizz {
  width: 100%;
  height: 100%;
}
.title {
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  text-align: center;
  color: white;
  opacity: 0.7;
  font-family: 'Knewave', cursive;
  font-size: 84px;
}
#fk{
    width:50%; 
    height:100%; 
    position:absolute;
    left:350px; top:700px; 
    border:; }
</style>
</head>
<body>
  <div class="row">
    <nav>
            <ul class="nav-ul">
              <li class="link"><a href="/bheema/">主页</a></li>
              <li class="link"><a href="/bheema/about/">关于</a></li>
              <li class="link"><a href="/bheema/archive/">文章</a></li>
            <li class="link"><a href="/bheema/contact/">联系</a></li>
  </ul>
    </nav>
</div>

<h1 class='title'>
  文章目录
</h1>

<canvas id="fizz">
</canvas>


<script>
(function() {
  this.Bubble = (function() {
    class Bubble {
      constructor(x, y, r, createdAt) {
        this.x = x;
        this.y = y;
        this.r = r;
        this.createdAt = createdAt;
      }

      velocity() {
        return this.r / 20 * Bubble.MAX_V;
      }

      grow(now) {
        if (!this.rising && this.r <= Bubble.MAX_R) {
          return this.r += Bubble.GROWTH_RATE * (now - this.createdAt);
        }
      }

      move(now) {
        if (this.rising) {
          return this.y -= (now - this.startedRisingAt) * this.velocity();
        }
      }

      rise() {
        if (!this.rising && this.r > 2) {
          this.rising = Math.random() < 0.15 * (this.r / Bubble.MAX_R);
          if (this.rising) {
            return this.startedRisingAt = new Date().getTime();
          }
        }
      }

    };

    Bubble.MAX_R = 20;

    Bubble.MAX_V = 0.02;

    Bubble.GROWTH_RATE = 0.00005;

    return Bubble;

  }).call(this);

  this.RisingBubbles = (function() {
    var rand, randInt;

    class RisingBubbles {
      constructor(id, maxBubbles) {
        var elem, i, j, ref;
        this.maxBubbles = maxBubbles;
        this.canvas = document.getElementById(id);
        elem = $('#' + id);
        elem.css('background-color', '#3d8835');
        elem.click(() => {
          var b, j, len, ref, results, ts;
          ts = new Date().getTime();
          ref = this.bubbles;
          results = [];
          for (j = 0, len = ref.length; j < len; j++) {
            b = ref[j];
            if (!b.rising) {
              b.rising = true;
              results.push(b.startedRisingAt = ts);
            } else {
              results.push(void 0);
            }
          }
          return results;
        });
        this.canvas.width = this.canvas.clientWidth;
        this.canvas.height = this.canvas.clientHeight;
        this.ctx = this.canvas.getContext('2d');
        this.ctx.fillStyle = '#FFFFFF';
        this.bubbles = [];
        this.lastFrame = new Date().getTime();
        for (i = j = 1, ref = randInt(0, this.maxBubbles); 1 <= ref ? j <= ref : j >= ref; i = 1 <= ref ? ++j : --j) {
          this.bubbles.push(new Bubble(randInt(0, this.canvas.width), randInt(0, this.canvas.height), rand(0, Bubble.MAX_R), new Date().getTime()));
        }
      }

      draw() {
        return this.run(new Date().getTime());
      }

      run(now) {
        var bubble, j, len, ref;
        this.update(now);
        this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
        ref = this.bubbles;
        for (j = 0, len = ref.length; j < len; j++) {
          bubble = ref[j];
          this.ctx.moveTo(bubble.x, bubble.y);
          this.ctx.beginPath();
          this.ctx.arc(bubble.x, bubble.y, bubble.r, 0, 2 * Math.PI);
          this.ctx.fill();
        }
        return requestAnimationFrame(() => {
          return this.run(new Date().getTime());
        });
      }

      update(now) {
        var b, i, j, k, len, ref, ref1, results;
        ref = this.bubbles;
        for (j = 0, len = ref.length; j < len; j++) {
          b = ref[j];
          b.grow(now);
          b.rise();
          b.move(now);
        }
        this.bubbles = (function() {
          var k, len1, ref1, results;
          ref1 = this.bubbles;
          results = [];
          for (k = 0, len1 = ref1.length; k < len1; k++) {
            b = ref1[k];
            if (b.y + b.r >= 0) {
              results.push(b);
            }
          }
          return results;
        }).call(this);
        if (this.maxBubbles - this.bubbles.length > 0) {
          results = [];
          for (i = k = 1, ref1 = randInt(0, this.maxBubbles - this.bubbles.length); 1 <= ref1 ? k <= ref1 : k >= ref1; i = 1 <= ref1 ? ++k : --k) {
            results.push(this.bubbles.push(new Bubble(randInt(0, this.canvas.width), randInt(0, this.canvas.height), 1, new Date().getTime())));
          }
          return results;
        }
      }

    };

    randInt = function(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    };

    rand = function(min, max) {
      return Math.floor(Math.random() * (max - min) + min);
    };

    return RisingBubbles;

  }).call(this);

  $((function() {
    var fizz;
    fizz = new RisingBubbles('fizz', 500);
    return fizz.draw();
  }));

}).call(this);

</script>

<div style="text-align:center;margin:50px 0; font:normal 14px/24px 'MicroSoft YaHei';">

<section id="archive">

{% for post in site.posts %}
  {% unless post.next %}
  <ul class="this">
  {% else %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
  {% if year != nyear %}
  </ul>
  <h2>{{ post.date | date: '%Y' }}</h2>
  <ul class="past">
  {% endif %}
  {% endunless %}
 <li class="arch-list" style="font-size: 20px"><a href="{{site.baseurl}}{{ post.url }}">{{ post.title }}</a>&nbsp;<time>{{ post.date | date:"%d %b" }}</time></li>
{% endfor %}
  </ul>
</section>
</div>
</body>
</html>











