### Froala Integration on Rails App:

Add the Gem on **Gemfile**, also install the **jquery-rails** gem to load jQuery features:
<pre>
  gem 'wysiwyg-rails'
  gem 'jquery-rails'
</pre>

Download the files from [**Froala Editor**](https://www.froala.com/wysiwyg-editor/download) and store them inside **assets** folder. 

On **application.js** file add the next lines:
<pre>
  // Include other plugins.

  //= require jquery3
  //= require froala_editor.min.js
  //= require froala_editor.pkgd.min.js

  //= require plugins/align.min.js
  //= require plugins/char_counter.min.js
  //= require plugins/code_beautifier.min.js
  //= require plugins/code_view.min.js
  //= require plugins/colors.min.js
  //= require plugins/emoticons.min.js
  //= require plugins/entities.min.js
  //= require plugins/file.min.js
  //= require plugins/font_family.min.js
  //= require plugins/font_size.min.js
  //= require plugins/fullscreen.min.js
  //= require plugins/help.min.js
  //= require plugins/image.min.js
  //= require plugins/image_manager.min.js
  //= require plugins/inline_style.min.js
  //= require plugins/line_breaker.min.js
  //= require plugins/link.min.js
  //= require plugins/lists.min.js
  //= require plugins/paragraph_format.min.js
  //= require plugins/paragraph_style.min.js
  //= require plugins/print.min.js
  //= require plugins/quick_insert.min.js
  //= require plugins/quote.min.js
  //= require plugins/save.min.js
  //= require plugins/table.min.js
  //= require plugins/special_characters.min.js
  //= require plugins/url.min.js
  //= require plugins/video.min.js

  //= require third_party/embedly.min.js
  //= require third_party/image_aviary.min.js
  //= require third_party/spell_checker.min.js

  //= require_tree .

  $('#post_content').froalaEditor();
</pre>

On **application.scss** file add the next lines:
<pre>
  @import 'froala_editor.min';
  @import 'froala_style.min';
  @import 'font-awesome-sprockets';
  @import 'font-awesome';

  @import 'plugins/char_counter.min.css';
  @import 'plugins/code_view.min.css';
  @import 'plugins/colors.min.css';
  @import 'plugins/emoticons.min.css';
  @import 'plugins/file.min.css';
  @import 'plugins/fullscreen.min.css';
  @import 'plugins/help.min.css';
  @import 'plugins/image_manager.min.css';
  @import 'plugins/image.min.css';
  @import 'plugins/line_breaker.min.css';
  @import 'plugins/quick_insert.min.css';
  @import 'plugins/special_characters.min.css';
  @import 'plugins/table.min.css';
  @import 'plugins/video.min.css';
</pre>

Now, on **Post Form Views** we need to insert the next code to set the **jQuery** callback to replace the textarea from the **Froala Editor**:
<pre>
  &lt;%= form_with(model: post, local: true) do |form| %&gt;
    &lt;% if post.errors.any? %&gt;
      &lt;div id="error_explanation"&gt;
        &lt;h2&gt;&lt;%= pluralize(post.errors.count, "error") %&gt; prohibited this post from being saved:&lt;/h2&gt;
        &lt;ul&gt;
        &lt;% post.errors.full_messages.each do |message| %&gt;
          &lt;li&gt;&lt;%= message %&gt;&lt;/li&gt;
        &lt;% end %&gt;
        &lt;/ul&gt;
      &lt;/div&gt;
    &lt;% end %&gt;
    &lt;div class="field"&gt;
      &lt;%= form.label :name %&gt;
      &lt;%= form.text_field :name, id: :post_name %&gt;
    &lt;/div&gt;
    &lt;div class="field"&gt;
      &lt;%= form.label :content %&gt;
      &lt;%= form.text_area :content, id: :post_content %&gt;
    &lt;/div&gt;
    &lt;div class="actions"&gt;
      &lt;%= form.submit %&gt;
    &lt;/div&gt;
  &lt;% end %&gt;
  <b>
  &lt;script&gt;
    $(function() {
      $('#post_content').froalaEditor({toolbarInline: false});
    });
  &lt;/script&gt;
  </b>
</pre>

After that, we need to set a code snippet to activate the **embedly plugin** which is stored inside the assets. And also, add the file **platform-embed.js** which is stored inside **third_party**, to the **config/initializers** folder and add the next line:
<pre>
  Rails.application.config.assets.precompile += %w( third_party/platform-embedly.js )
</pre>

<pre>
  &lt;p id="notice"&gt;&lt;%= notice %&gt;&lt;/p&gt;
  &lt;p&gt;
    &lt;strong&gt;Name:&lt;/strong&gt;
    &lt;%= @post.name %&gt;
  &lt;/p&gt;
  &lt;p&gt;
    &lt;strong&gt;Content:&lt;/strong&gt;
    &lt;%= @post.content.html_safe %&gt;
  &lt;/p&gt;
  &lt;%= link_to 'Edit', edit_post_path(@post) %&gt; |
  &lt;%= link_to 'Back', posts_path %&gt;
  <b>
  &lt;script&gt;
    (function(w, d){
     var id='embedly-platform', n = 'script';
     if (!d.getElementById(id)){
       w.embedly = w.embedly || function() {(w.embedly.q = w.embedly.q || []).push(arguments);};
       var e = d.createElement(n); e.id = id; e.async=1;
       e.src = ('https:' === document.location.protocol ? 'https' : 'http') + '://cdn.embedly.com/widgets/platform.js';
       var s = d.getElementsByTagName(n)[0];
       s.parentNode.insertBefore(e, s);
     }
    })(window, document);
  &lt;/script&gt;
  </b>
  OR
  <b>
  &lt;script&gt;
    (function(w, d){
     var id='embedly-platform', n = 'script';
     if (!d.getElementById(id)){
       w.embedly = w.embedly || function() {(w.embedly.q = w.embedly.q || []).push(arguments);};
       var e = d.createElement(n); e.id = id; e.async=1;
       e.src = ('https:' === document.location.protocol ? 'https' : 'http') + &lt;%= asset_path('third_party/platform-embedly.js') %&gt;;
       var s = d.getElementsByTagName(n)[0];
       s.parentNode.insertBefore(e, s);
     }
    })(window, document);
  &lt;/script&gt;
  </b>
</pre>
