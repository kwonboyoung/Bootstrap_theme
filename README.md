## asset pipeline

## c9) bootstrap_theme

```ruby
# gemfile
gem 'faker'
gem 'font-awesome-rails'
gem 'kaminari'
gem 'devise'
```

```ruby
$ bundle install
$ rails g scaffold posts title:string contents:text
$ rails g devise:install
$ rails g devise user
$ rake db:migrate
```

```ruby
# routes.rb
root 'posts#index'
```



https://startbootstrap.com/template-overviews/clean-blog/ 템플릿 다운 후

app >> assets >> stylesheets >> application.css 를 application.scss라고 수정

vendor >> assets >> javascripts 에 bootstrap.bundle.js, bootstrap.js 복사 붙이기

vendor >> assets >> stylesheets에 bootstrap-grid.css, bootstrap-reboot.css, bootstrap.css 복사 붙이기

app >> assets >> images  받은 템플릿 img 폴더 안의 이미지들 복사 붙이기

app >> assets >> stylesheets  clean-blog.css 복사 붙이기



* assets에서 제일 중요한 3가지

이미지 태그, 폰트 태그, 정확한 파일 위치

```ruby
# app >> assets >> stylesheets >> application.scss
@import 'bootstrap';
@import 'font-awesome';
@import 'bootstrap-reboot';
@import 'bootstrap-grid';
@import 'clean-blog';
```

```ruby
# views >> layouts >> _footer.html.erb 생성 (받은 템플릿의 index.html코드 긁어옴)
<footer>
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        <ul class="list-inline text-center">
          <li class="list-inline-item">
            <a href="#">
              <span class="fa-stack fa-lg">
                <i class="fa fa-circle fa-stack-2x"></i>
                <i class="fa fa-twitter fa-stack-1x fa-inverse"></i>
              </span>
            </a>
          </li>
          <li class="list-inline-item">
            <a href="#">
              <span class="fa-stack fa-lg">
                <i class="fa fa-circle fa-stack-2x"></i>
                <i class="fa fa-facebook fa-stack-1x fa-inverse"></i>
              </span>
            </a>
          </li>
          <li class="list-inline-item">
            <a href="#">
              <span class="fa-stack fa-lg">
                <i class="fa fa-circle fa-stack-2x"></i>
                <i class="fa fa-github fa-stack-1x fa-inverse"></i>
              </span>
            </a>
          </li>
        </ul>
        <p class="copyright text-muted">Copyright &copy; BY Website 2017</p>
      </div>
    </div>
  </div>
</footer>
            
# views >> layouts >> _nav.html.erb 생성 (받은 템플릿의 index.html코드 긁어옴)
<nav class="navbar navbar-expand-lg navbar-light fixed-top" id="mainNav">
  <div class="container">
    <a class="navbar-brand" href="index.html">Start Bootstrap</a>
    <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
      Menu
      <i class="fa fa-bars"></i>
    </button>
    <div class="collapse navbar-collapse" id="navbarResponsive">
      <ul class="navbar-nav ml-auto">
        <li class="nav-item">
          <a class="nav-link" href="index.html">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="about.html">About</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="post.html">Sample Post</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="contact.html">Contact</a>
        </li>
      </ul>
    </div>
  </div>
</nav>
              
# views >> layouts >> application.html.erb
<%= render "/layouts/nav"%>
<%= yield %>
<%= render "/layouts/footer"%>
```

```ruby
# _nav.html.erb 하나씩 link_to로 고치기
<nav class="navbar navbar-expand-lg navbar-light fixed-top" id="mainNav">
  <div class="container">
    <!--<a class="navbar-brand" href="index.html">Start Bootstrap</a>-->
    <%= link_to "LikeLion", root_path, class: "navbar-brand"%>
    <button class="navbar-toggler navbar-toggler-right" type="button" data-toggle="collapse" data-target="#navbarResponsive" aria-controls="navbarResponsive" aria-expanded="false" aria-label="Toggle navigation">
      Menu
      <i class="fa fa-bars"></i>
    </button>
    <div class="collapse navbar-collapse" id="navbarResponsive">
      <ul class="navbar-nav ml-auto">
        <li class="nav-item">
          <%= link_to "Home", root_path, class: "nav-link"%>
        </li>
        <li class="nav-item">
          <%= link_to "New Post", new_post_path, class: "nav-link"%>
        </li>
        <li class="nav-item">
          <%if user_signed_in? %>
              <%= link_to "logout", destroy_user_session_path, method: :delete, data: {confirm: "정말 로그아웃하시겠습니까?"}, class: "nav-link"%>
          <%else%>
              <%= link_to "login", new_user_session_path, class: "nav-link"%>
          <%end%>
        </li>
      </ul>
    </div>
  </div>
</nav>
```

```ruby
# views >> posts >> index.html.erb (받은 템플릿의 index.html코드 긁어옴)
# image: url 고쳐주기
<header class="masthead" style="background-image: url(<%= asset_path 'home-bg.jpg'%>)">
  <div class="overlay"></div>
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        <div class="site-heading">
          <h1>LikeLion</h1>
          <span class="subheading">Bootstrap theme class</span>
        </div>
      </div>
    </div>
  </div>
</header>

<!-- Main Content -->
<div class="container">
  <div class="row">
    <div class="col-lg-8 col-md-10 mx-auto">
      <% @posts.each do |post|%>
      <div class="post-preview">
        <a href="<%= post_path(post)%>">
          <h2 class="post-title">
            <%=post.title%>
          </h2>
          <h3 class="post-subtitle">
            <%= post.contents%>
          </h3>
        </a>
        <p class="post-meta">Posted by
          <a href="<%= post_path(post)%>">Start Bootstrap</a>
          on <%= post.created_at.to_s(:long)%></p>
      </div>
      <%end%>
      <hr>
    </div>
  </div>
</div>
```

```ruby
# seeds.rb
1000.times do
    Post.create(
        title: Faker::Name.title,
        contents: Faker::Lorem.paragraphs
    )
end
```

```ruby
$ rake db:seed
```

app >> assets >> js >> clean-blog.js, contact_me.js, jqBootstrapValidation.js 추가

```ruby
# app >> assets >> js >> application.js
# template에 들어있는 순서로 적는 것이 중요하다. (순서는 contact.html 참고)
//= require bootstrap
//= require bootstrap.bundle
//= require jqBootstrapValidation
//= require contact_me
//= require clean-blog
```

```ruby
# application.html.erb
# head 부분에 추가(웹 페이지 줄이고 늘릴 때 중요한 역할을 하므로 꼭 넣어주도록)
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

```ruby
# views >> posts >> show.html.erb (받은 템플릿의 post.html코드 긁어옴)
<!-- Page Header -->
<header class="masthead" style="background-image: url(<%= asset_path 'post-bg.jpg'%>)">
  <div class="overlay"></div>
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        <div class="post-heading">
          <h1>Man must explore, and this is exploration at its greatest</h1>
          <h2 class="subheading">Problems look mighty small from 150 miles up</h2>
          <span class="meta">Posted by
            <a href="#">Start Bootstrap</a>
            on <%= @post.created_at.to_s(:short)%></span>
        </div>
      </div>
    </div>
  </div>
</header>

<!-- Post Content -->
<article>
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">

        <h2 class="section-heading">The Final Frontier</h2>

        <blockquote class="blockquote">The dreams of yesterday are the hopes of today and the reality of tomorrow. Science has not yet mastered prophecy. We predict too much for the next year and yet far too little for the next ten.</blockquote>

        <%= @post.contents%>
      
        <a href="#">
          <img class="img-fluid" src="<%= asset_path 'post-sample-image.jpg'%>" alt="">
        </a>
      </div>
    </div>
  </div>
</article>

<hr>
```

```ruby
# views >> posts >> _form.html.erb (받은 템플릿의 contact.html코드 긁어옴)
<!-- Page Header -->
<header class="masthead" style="background-image: url(<%= image_url 'contact-bg.jpg'%>)">
  <div class="overlay"></div>
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        <div class="page-heading">
          <h1>New Post</h1>
          <span class="subheading">Create a new Post</span>
        </div>
      </div>
    </div>
  </div>
</header>

<!-- Main Content -->
<div class="container">
  <div class="row">
    <div class="col-lg-8 col-md-10 mx-auto">
      <%= form_tag "", id: "contactForm", novalidate: "" do%>
        <div class="control-group">
          <div class="form-group floating-label-form-group controls">
            <label>Title</label>
            <!-- 두번째 파라미터는 value를 넣어줘야 하는데 우린 필요가 없으므로 ""를 넣어줌 -->
            <%= text_field_tag "posts[title]", "", class: "form-control", placeholder: "Title"%>
            <p class="help-block text-danger"></p>
          </div>
        </div>
        <div class="control-group">
          <div class="form-group floating-label-form-group controls">
            <label>Message</label>
            <textarea rows="5" class="form-control" placeholder="Message" id="message" required data-validation-required-message="Please enter a message."></textarea>
            <p class="help-block text-danger"></p>
          </div>
        </div>
        <br>
        <div id="success"></div>
        <div class="form-group">
          <button type="submit" class="btn btn-primary" id="sendMessageButton">Send</button>
        </div>
      <%end%>
    </div>
  </div>
</div>

<hr>


<%= form_for(@post) do |f| %>
  <% if @post.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(@post.errors.count, "error") %> prohibited this post from being saved:</h2>

      <ul>
      <% @post.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

<% end %>
```

