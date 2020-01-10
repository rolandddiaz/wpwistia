# wpwistia
Welcome to Wistia API Programming


# iTunes, ahem - http://embed.wistia.com/deliveries/856970d9a4bcb9aab381a0bd9ab714f19d72c62f.bin
# Windows and linix - http://embed.wistia.com/deliveries/856970d9a4bcb9aab381a0bd9ab714f19d72c62f/my-file.mp4
# API and ID Binder - https://videolinkfor.glitch.me/ 

async API embed
<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script>
<div class="wistia_embed wistia_async_g5pnf59ala videoFoam=true" style="width:640px;height:360px;">&nbsp;</div>

# First, we have an async script tag for E-v1.js.
<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script>

# 2nd is a video container and class
<div class="wistia_embed wistia_async_g5pnf59ala videoFoam=true" style="width:640px;height:360px;">&nbsp;</div>

# This is a "wistia_ember wistia_async_" Class 
# This is a wistia_async_hashedid class "g5pnf59ala"
# Third is option value "videoFoam=true"
# Fourth, the style attribute "style="width:640px;height:360px;""
# Last, the contents of the container.
<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async>
	
</script><div class="wistia_embed wistia_async_g5pnf59ala videoFoam=true" style="width:640px;height:360px;">  &nbsp;</div>

# Using the Wistia library asynchronously

Because E-v1.js is loaded asynchronously, we need to make sure any code that references it only runs after window.Wistia is defined. There are a few ways to do that.

The simplest way is as follows:

window.wistiaInit = function(W) {console.log("Wistia library loaded and available in the W argument!");};

we recommend using the following form instead:

window.wistiaInitQueue = window.wistiaInitQueue || [];
window.wistiaInitQueue.push(function(W) {
  console.log("Wistia library loaded and available in the W argument!");
});

window.wistiaInitQueue functions the same way as window.wistiaInit except it will not clobber any other functions that have been defined to run on initialization.

# Getting an API handle
Traditional API embeds set the wistiaEmbed variable right in the embed code. We can’t do that with async embeds, so we need to use a new method: the Wistia.api() function.

<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script>
<div id="my_video" class="wistia_embed wistia_async_g5pnf59ala" style="width:640px;height:360px;">&nbsp;</div>
<script>
window.wistiaInit = function(W) {
  W.api("my_video").play();
};
</script>

# In that snippet, we defined an ID attribute on the video container, then referenced that ID after the Wistia library was initialized.
# Now that you have a handle to the video, you can use it just like a traditional API embed, with the Player API.

# Setting embed options
The option=value method is very simple if you only need to make a few small adjustments. But if you want to set more complex inline options, it gets a little clunky. In that case, you might want to use the Wistia.options function.

Note that these options only affect videos that are embedded after Wistia.options() is called. Videos that are already embedded will not be affected.

<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script>
<div id="my_video" class="wistia_embed wistia_async_g5pnf59ala" style="width:640px;height:360px;">&nbsp;</div>
<script>
window.wistiaInit = function(W) {
  W.options("my_video", {
    autoPlay: true,
    plugin: {
      "socialbar-v1": {
        buttons: "facebook-twitter"
      }
    }
  });
};
</script>


# The setup for this is similar to getting an API handle. We define an ID attribute on the video container, then reference it in the options function.

<script charset="ISO-8859-1" src="//fast.wistia.com/assets/external/E-v1.js" async></script>
<div id="my_video" class="wistia_embed wistia_async_g5pnf59ala" style="width:640px;height:360px;">&nbsp;</div>
<script>
window.wistiaInit = function(W) {
  W.options("my_video", {
    autoPlay: true,
    plugin: {
      "socialbar-v1": {
          buttons: "facebook-twitter"
      }
    }
  });
};
</script>


# You can also set options globally for all embeds on a page by not specifying an ID. For example, I can say that all videos on this page should have a red playerColor:

window.wistiaInit = function(W) {
  W.options({ playerColor: "ff0000" });
};

# How async embeds are initialized (for the curious)

The Wistia library defines Wistia.embeds.setup() which finds all (or a subset of) async video containers on the page and initializes them as Wistia videos. This function is called as soon as E-v1.js loads for the best experience. Once E-v1.js is loaded, it will watch the DOM forever for new Wistia embeds.

To setup that watch, we use a feature of modern browsers called mutation observers. They allow us to efficiently detect whenever a <div> with a wistia_embed and wistia_async_hashedid class is added to the DOM. Older browsers don’t have mutation observers, so in those, we poll the DOM once every 500ms for any matching elements that have not been initialized.

The following information is for micro-optimization on older browsers. Going forward, this will be less and less important, so we advise only making these micro-optimizations if you have no other choice.

If you want to prevent any initialization lag in older browsers, you can call Wistia.embeds.setup() immediately after you inject a Wistia embed into the page.

If you have a site with an extremely large number of <div> elements and are optimizing for older browsers, you might want to turn off our automatic watch. To do that, you can add this javascript:

window.wistiaInitQueue = window.wistiaInitQueue || [];
window.wistiaInitQueue.push(function(W) { W.embeds.dontWatch(); });
If you choose to turn off the watch, then you will need to call Wistia.embeds.setup() manually whenever you inject a new Wistia embed into the DOM.
