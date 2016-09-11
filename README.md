#■Search Lab - 蘇介吾 105/09/11

##功能：讓搜尋關鍵字(Keyword)可以同時找尋多個欄位(Name、Price)

1. rails new search
2. cd search
3. rails g scaffold Products name price:integer
4. rake db:migrate
5. vi config/routes.rb 加入
```
get 'search' => 'products#search'
root 'products#search'
```
vi app/controllers/products_controller.rb 加入
```
def search
end

def index
  if params[:keyword]
    #@projects = Project.where("name LIKE ?", "#{params[:keyword]}")  //單欄搜尋
    @projects = Project.where("name LIKE :keyword OR price LIKE :keyword", keyword: "%#{params[:keyword]}%")  //多欄搜尋
    @keyword = params[:keyword]
  else
    @projects = Project.all
  end
end
```
vi app/views/search.html.erb
```
<h1>Search</h1>
<%= form_tag projects_path, method: 'get' do %>
  <p>
    <%= text_field_tag :keyword, params[:keyword] %>
    <%= submit_tag "Search", name: nil %>
  </p>
<% end %>
```
vi app/views/index.html.erb
```
<%= "Search Keyword is '#{@keyword}'<br><br>".html_safe unless @keyword.empty? %>
```

![Demo](https://github.com/afgnsu/search/blob/master/DEMO.png)
![Demo1](https://github.com/afgnsu/search/blob/master/DEMO1.png)
![Demo2](https://github.com/afgnsu/search/blob/master/DEMO2.png)
![Demo3](https://github.com/afgnsu/search/blob/master/DEMO3.png)
