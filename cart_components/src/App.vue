<template>
  <div id="cart_components">
    <shop></shop>
    <section id="topParallax" data-type="background" data-speed="10" class="pages parallax">
      <article data-type="text" data-speed="5" class="parallax">I am absolute positioned</article>
    </section>
    <section id="bottomParallax" data-type="background" data-speed="5" class="pages parallax">
      <article data-type="text" data-speed="5" class="parallax">Simple Parallax Scroll</article>
    </section>
  </div>
</template>

<script>
  import Shop from './components/Shop.vue';
  import jQuery from '../node_modules/jquery/dist/jquery.min.js';

  export default {
    name: 'cart_components',
    data() {
        return {

        };
    },
    components: { Shop },
  }

  jQuery(document).ready(function(){
    jQuery('.parallax[data-type="background"]').each(function(){
      var bgobj = jQuery(this);

      jQuery(window).scroll(function(){
        var yPos = -(jQuery(window).scrollTop() / bgobj.data('speed'));

        var coords = '50% ' + yPos + 'px';

        bgobj.css('background-position', coords);
      });
    });

    jQuery('.parallax[data-type="text"]').each(function(){
      var textobj = jQuery(this);
      var originalTop = textobj.position().top;

      jQuery(window).scroll(function(){
        var topPos = -((jQuery(window).scrollTop()) / textobj.data('speed'));

        var coords = topPos + originalTop + 'px';

        textobj.css('top', coords);
      });
    });
  });
</script>

<style lang="scss">
#cart_components {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position:relative;
}

#topParallax, #bottomParallax{
  background:url('images/blue.jpg') repeat fixed;
  background-position:50% 0;
  min-height:800px;
  height:800px;
  margin:0 auto;
  width:100%;
  position:relative;
}

#bottomParallax{
  background:url('images/cat.jpg') no-repeat fixed;
  background-position:50% 0;
  -webkit-box-shadow: 0 0 50px rgba(0,0,0,0.8);
  box-shadow: 0 0 50px rgba(0,0,0,0.8);
}

#topParallax article, #bottomParallax article{
  height:400px;
  position:absolute;
  text-align:center;
  top:300px;
  width:100%;
  font-size:40px;
  color:#FFF;
  text-shadow: 0 0 3px #000;
}


</style>
